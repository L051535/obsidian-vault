 Add write capability to the Veeva Vault MCP connector

     Context

     server.py (v1.9.2, 26 tools) was deliberately hardened to be read-only:
     _ReadOnlyAsyncClient (server.py:137-159) blocks every HTTP verb except
     GET/HEAD before any socket I/O, with exactly one narrow exemption — POST to
     the VQL /query endpoint (VQL is SELECT-only, so it's read-only despite using
     POST). This was a tested security control (tests/test_hardening.py), not an
     oversight.

     The user now wants to create new Deviation records, update fields on
     existing Deviations, and create CAPA/Investigation/Root-Cause child records
     linked to a Deviation — real write capability against a GxP-regulated Quality
     system. Per explicit decision, writes will be on by default — no env-var
     opt-in gate. The safety boundary is instead the same "narrow, explicit,
     tested allow-list" pattern the codebase already uses elsewhere (VQL-POST-only
     exemption, _VAULT_HOSTNAME allow-list on _vault_get_url against
     cursor-based token exfiltration, the strict 401-vs-403 error split): writes
     are permitted only to five specific Quality objects, via URL-shape checks
     enforced at the transport layer itself — never a blanket "writes on" switch.

     Out of scope (not requested, flagged for a future pass if ever needed):
     effectiveness-check writes, workflow/e-signature actions, record deletion,
     and updates to CAPA/Investigation/Root-Cause records (only creation of
     those was requested).

     Relationship model (confirmed by reading server.py:1954-2110)

     ┌────────────────────────┬────────────────────────────────────────────────────────────────────────────────────────┐
     │      Child object      │                                     Link mechanism                                     │
     ├────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
     │ capa_action__qdm (CAPA │ direct FK deviation__v on the CAPA record, OR a capa_deviation__v junction row         │
     │  Action)               │ (capa_action__v, deviation__v) — code comment says either may be used depending how    │
     │                        │ the CAPA was created                                                                   │
     ├────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
     │ investigation__qdm     │ direct FK deviation__v only, no junction                                               │
     ├────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
     │ root_cause__qdm        │ direct FK deviation__v only, no junction                                               │
     └────────────────────────┴────────────────────────────────────────────────────────────────────────────────────────┘

     Default: direct FK only (simpler, always works). create_capa_action gets an
     optional also_create_junction flag to additionally write the junction row —
     verify against the org's actual Vault process which convention it expects.

     1. Transport-layer allow-list (server.py, near line 126-159)

     Add, next to _VQL_QUERY_URL:

     _WRITABLE_OBJECTS = frozenset({
         "deviation__v", "capa_action__qdm", "investigation__qdm",
         "root_cause__qdm", "capa_deviation__v",
     })
     _CREATE_URL_RE = re.compile(r"^" + re.escape(VAULT_API_BASE) + r"/vobjects/([A-Za-z][A-Za-z0-9_]*)/?$")
     _UPDATE_URL_RE = re.compile(r"^" + re.escape(VAULT_API_BASE) +
     r"/vobjects/([A-Za-z][A-Za-z0-9_]*)/([A-Za-z0-9]+)$")

     Extend _ReadOnlyAsyncClient.request(): after the existing VQL exemption
     check, add — for POST, match _CREATE_URL_RE and require
     group(1) in _WRITABLE_OBJECTS; for PUT (new verb for this codebase),
     match _UPDATE_URL_RE and require group(1) in _WRITABLE_OBJECTS. Anchored
     regexes on the full URL string (not startswith) — mirrors the exactness of
     the existing _VQL_QUERY_URL == check, rejects path-traversal/query-string
     smuggling. PATCH and DELETE get no exemption path at all, ever. stream() is
     untouched (GET/HEAD-only forever — no write tool needs it).

     Update ReadOnlyViolation's docstring/remediation text — it currently claims
     "structurally impossible" for all non-GET, which is no longer literally true;
     reword to describe the allow-list model.

     2. New internal helpers (near _vault_get_objects, line 424-432)

     async def _vault_create(object_name: str, fields: dict) -> dict:
         if object_name not in _WRITABLE_OBJECTS:
             raise ValidationError(f"{object_name} is not a writable object.")
         token = await _require_token()
         resp = await _http.post(f"{VAULT_API_BASE}/vobjects/{object_name}", data=fields,
             headers={"Content-Type": "application/x-www-form-urlencoded",
                      "Accept": "application/json", "Authorization": token})
         return _parse_response(resp)

     async def _vault_update(object_name: str, vault_id: str, fields: dict) -> dict:
         if object_name not in _WRITABLE_OBJECTS:
             raise ValidationError(f"{object_name} is not a writable object.")
         token = await _require_token()
         resp = await _http.put(f"{VAULT_API_BASE}/vobjects/{object_name}/{vault_id}", data=fields,
             headers={"Content-Type": "application/x-www-form-urlencoded",
                      "Accept": "application/json", "Authorization": token})
         return _parse_response(resp)

     Reuse _require_token(), _parse_response(), the existing header triple —
     _vault_err/_classify_vault_error (line 1299-1333) work unmodified since
     they only key off responseStatus/errors. Vault's real create/update
     response shape has never been observed in this codebase — extract the new
     id defensively: new_id = result.get("id") or (result.get("data") or {}).get("id", ""),
     and confirm/adjust against a live call before calling this tool "done" (see
     verification checklist below).

     3. New @mcp.tool() functions

     Generic gateway pair:
     - vault_create_record(object_name: str, fields: dict) -> str
     - vault_update_record(object_name: str, identifier: str, fields: dict) -> str
     (reuses existing _resolve_record_id(object_name, identifier), line 2268-2286)

     Purpose-built wrappers (named params for common fields + fields: dict | None
     escape hatch, same pattern search_capa_actions's where param already
     uses):
     - create_deviation(title, description, deviation_type="", impact="", owning_facility="", department="",
     date_occurred="", fields=None) -> str
     - update_deviation(identifier: str, fields: dict) -> str
     - create_capa_action(deviation_identifier, title, action_type="", description="", also_create_junction=False,
     fields=None) -> str
     - create_investigation(deviation_identifier, title, investigation_type="", fields=None) -> str
     - create_root_cause(deviation_identifier, cause_description, root_cause_type="", fields=None) -> str

     All resolve deviation_identifier/identifier via the existing
     _resolve_dev_name/_resolve_record_id helpers — no new resolution logic.
     create_capa_action with also_create_junction=True creates the CAPA first,
     then attempts the junction row as a second step; if the junction write
     fails, the CAPA is NOT rolled back (Vault's REST API has no cross-object
     transaction) — report both outcomes in the returned JSON
     ({"capa_action": {...}, "junction": {...} | null}).

     Match existing docstring style exactly: Args:/Returns: blocks, "Requires
     SSO login — call vault_login first if needed." verbatim.

     Tool count: 26 → 33 (7 new tools, always registered — no conditional
     gating).

     4. Validation

     New taxonomy member (server.py:61-124 block), NOT a PermissionError
     subclass (unlike NotAuthenticated/AccessDenied/UntrustedHost — this is
     a shape/contract failure, not auth):

     class ValidationError(ConnectorError):
         error_class = "ValidationError"
         remediation = "Fix the request parameters (object name, field names/values) and retry."
         owner = "caller"

     Validate client-side (fast-fail, no network call): object name against
     _WRITABLE_OBJECTS and _OBJECT_NAME_RE (existing, line 2265); non-empty
     fields dict; date-shaped params via existing _valid_date() (line
     549-553) — reuse, don't reinvent.

     Deliberately do NOT hardcode a required-fields list or replicate Vault's
     picklist/business-rule validation — let Vault's own API reject bad writes
     with its own error text, which flows through the existing
     _classify_vault_error generic-ConnectorError fallback (line 1324-1325)
     unchanged. vault_get_picklist/vault_describe_object remain
     discovery tools a caller can check beforehand, not enforcement gates.

     5. Tests (tests/test_hardening.py)

     New "R2: write allow-list" section, same style as existing (no
     pytest-asyncio, _run() helper, _no_leaked_token fixture, hits
     127.0.0.1:1/unreachable hosts — no real Vault contact):

     - POST/PUT to an allow-listed object URL reaches the transport (raises a
     transport error, not ReadOnlyViolation).
     - POST/PUT to a non-allow-listed object (e.g. documents, user__sys)
     still raises ReadOnlyViolation — the single most important test: proves
     the allow-list is doing the gating, not a blanket switch.
     - Malformed/mismatched write URLs (trailing slash, extra path segment, query
     string) still blocked — guards the regex-anchoring.
     - PATCH/DELETE always blocked regardless (split off from the existing
     parametrized test_readonly_client_blocks_non_get_pre_socket test, since
     POST/PUT are no longer unconditionally blocked).
     - ValidationError taxonomy test (subclass of ConnectorError, NOT
     PermissionError, non-empty owner/remediation) — mirrors
     test_taxonomy_classes_hierarchy_and_owners.
     - _vault_create/_vault_update reject a non-writable object name with
     ValidationError before touching _require_token()/network at all.
     - Bump TOOL_COUNT_BEFORE_HARDENING 26 → 33 in
     test_import_smoke_pinned_tool_count, with a comment listing the 7 new
     tool names.

     Manual/live verification checklist (run once against real Vault before
     shipping — ideally a sandbox, else a disposable test deviation):
     1. create_deviation(...) — inspect the raw response to confirm the actual
     id field shape (flat id vs nested data.id); fix extraction if needed.
     2. get_deviation(<new id>) — confirm round-trip field values.
     3. update_deviation(<id>, {...}) then get_deviation again — confirm the
     update landed.
     4. create_capa_action(..., also_create_junction=False) then
     get_deviation_capas(<id>) — confirm it shows up via direct FK.
     5. Repeat with also_create_junction=True — confirm it shows up via both FK
     and junction paths.
     6. create_investigation / create_root_cause similarly, verified via
     get_deviation_investigations/get_deviation_root_causes.
     7. Deliberately omit a field Vault requires — confirm the error surfaces as
     a clean ConnectorError message, not a raw stack trace.
     8. Ask the Vault/Quality process owner which CAPA-linking convention (direct
     FK, junction, or both) this org's business process actually expects, and
     confirm/flip the also_create_junction default accordingly.

     9. Documentation

     CHANGELOG.md: new ## [1.10.0] - 2026-08-11 — write capability entry,
     ### Security heading describing the narrow write allow-list (list the 5
     writable objects, note PATCH/DELETE remain permanently blocked, no
     env-var gate — writes on by default, allow-list is the sole boundary),
     ### Added heading listing the 7 new tools + ValidationError +
     _vault_create/_vault_update, ### Out of scope heading noting
     effectiveness-checks/workflow/deletion/child-record-updates were
     deliberately not added.

     pyproject.toml: version 1.9.2 → 1.10.0 (feature addition, matches
     project's own precedent of minor bumps for new tools).

     Verification

     10. pytest tests/ — all existing + new tests pass, including the updated
     tool-count pin (33) and the new R2 write-allow-list section.
     11. Manual live checklist above, run once against real Vault.
     12. Restart both Claude Desktop and Claude Code's MCP connection (or just
     Claude Code, which is already registered) and confirm
     server.mcp.list_tools() / the visible tool palette shows 33 tools
     including the 7 new ones.