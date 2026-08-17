---
date: 2026-08-17
status:
type:
tags:
  - "#claude/mcp"
  - "#veeva/ai"
---
 ═══════════════════════════════════════════════════════════════════
                           THE COUNCIL
     "Can Claude safely write deviations and CAPAs into Vault QMS?"
  ═══════════════════════════════════════════════════════════════════

  ⚔ THE ADVERSARY

  Your second premise is backwards, and it's the load-bearing one. Writing through the user's SSO token doesn't create
  accountability — it manufactures false attribution, which is strictly worse than no attribution. The audit trail will
  say a named human authored a root cause statement they skimmed for eleven seconds, and in an FDA inspection that's not
  a feature, it's the 483 observation: records not attributable to the person who actually performed the activity,
  ALCOA's "A" broken at the root. And look at what your own tool surface already admits — create_capa_action says that
  if the junction write fails after the CAPA is created, the CAPA is not rolled back, and it tells the model to "check
  an existing linked CAPA if unsure which convention this org uses." You have an unvalidated LLM guessing at your data
  model and leaving orphaned CAPA records in a GxP system. That's not a compliance gap, that's a self-documenting one.

  ──────────────────────────────────────────────────────────────────

  📈 THE STRATEGIST

  The Adversary is right about the mechanism and wrong about the conclusion — the fix is sequencing, not abandonment.
  The business case here is real and measurable: deviation cycle time and overdue-CAPA backlog are board-visible metrics
  at every large manufacturer, and 60–70% of an investigator's time on a routine deviation is transcription, not
  thinking. But you're attacking the highest-risk object class first. Risk-tier it: quality_incident__v and initial
  fact-finding are recoverable and low-consequence; CAPA effectiveness checks and closure records are inspection
  artifacts with regulatory weight. And the real leverage play isn't building this — it's making Veeva build it. Veeva
  shipped AI Agents into Vault in late 2025; Lilly is large enough to put "supported, validated AI write provenance on
  Quality objects" into a roadmap conversation and move the entire validation burden onto the vendor's quality system
  instead of yours.

  ──────────────────────────────────────────────────────────────────

  🔬 THE SCIENTIST

  Everyone is arguing about a system whose error rate nobody has measured. Two experiments, both cheap, both runnable
  this quarter. First: pull 200 closed deviations, have Claude draft the root cause and CAPA from the source evidence
  only, and have two QA reviewers blind-score drafts against the approved final on a fixed rubric — you need the rate of
  material error, not "reads well." Second, and this is the one that decides whether the confirmation interface is
  real: a seeded-error study. Inject 20 deliberately defective drafts into 100 and measure the reviewer catch rate. The
  automation-bias literature is unkind here — human monitors of automated recommendations routinely miss errors they
  would have caught if they'd generated the work themselves, and accept-rates stay high even when the automation is
  wrong. If your reviewers catch under 90% of seeded defects, the Humanist's rubber-stamp concern isn't a worry, it's
  your measured baseline, and the approval gate is decoration.

  ──────────────────────────────────────────────────────────────────

  🎨 THE VISIONARY

  You're fighting for write access to a validated database when what you actually want is to delete six hours of typing
  from an investigator's week — and those are not the same problem. Two reframes. One: point Claude the other direction.
  Instead of drafting records, have it critique human-drafted deviations against the SOP, the regulatory expectation,
  and the last five similar events — a red-team pass before submission. Read-only, zero Part 11 surface, and it attacks
  the thing that actually gets cited, which is weak investigations, not slow ones. Two: if you must produce content,
  produce a document, not a field write. Claude generates the investigation package as an attachment, and it enters
  through the controlled-document review path your company already validated a decade ago. The Adversary can't shoot at
  an architecture that has no new write path in it.

  ──────────────────────────────────────────────────────────────────

  ⚙ THE ENGINEER

  The Visionary's document path dodges the problem rather than solving it, but their instinct about where the boundary
  sits is correct — so here's where I'd actually draw it. Claude must never hold Vault write credentials. It emits a
  proposal payload: object API name, field API names, values, provenance, hashed. A separate thin reviewer app renders a
  field-level diff, the human edits and approves, and the app's own deterministic code path executes the write under
  the user's SSO token — with an assertion that the hash committed equals the hash approved. That single check kills the
  failure mode nobody plans for, where the model regenerates between review and commit. Three things to hard-block in
  that app: direct writes to status__v (a field-level state change bypasses the Vault workflow and the e-signature
  attached to it — that's a Part 11 authority-check failure, not a shortcut), multi-object writes without a compensating
  rollback (your junction problem), and any field value derived from read_record_attachment reaching a write without
  human diff review, because attachment text is uploaded by third parties and is a live prompt-injection path into a
  validated system. Then validate the harness, not the model — GAMP 5 second edition and the CSA guidance let you do
  exactly that if your intended-use statement says the model output is unverified draft data and the qualification
  boundary is the approval gate.

  ──────────────────────────────────────────────────────────────────

  🧘 THE PHILOSOPHER

  I want to name who isn't in this room. Not the inspector — the inspector is a proxy. It's the patient who receives the
  batch that the CAPA was supposed to protect, and it's the investigator sitting in an inspection interview three years
  from now being asked "did you write this?" and having to decide, under oath, what "write" means. The Strategist
  measures cycle time. I'd ask whether faster closure and better quality are the same variable or opposite ones, because
  a fluent, well-structured root cause is harder to challenge than a clumsy one — and a wrong root cause that nobody
  challenges is how a recurring failure becomes permanent. The Engineer's hash check is elegant and it verifies
  fidelity, not truth. There is a version of winning this where every deviation closes on time, the backlog goes to
  zero, the metrics all turn green, and nothing on the manufacturing floor actually gets safer. Decide now, on paper,
  which of those you're optimizing for — because the system you build will optimize for one of them whether you choose
  or not.

  ──────────────────────────────────────────────────────────────────

  ❤ THE HUMANIST

  The Philosopher's inspection interview is the thing I'd put in front of your QA leadership first, because it's not
  hypothetical — it's a specific person with a name whose professional credibility is on the line for text they didn't
  compose. Think about what this actually does to their day: instead of writing eight investigations they now approve
  forty, and approving forty is a fundamentally different cognitive task that nobody has trained them for or given them
  time for. Ask two questions before you build anything. Can a reviewer reject a draft and send it back without a
  cycle-time metric punishing them for it? And who owns the record when the draft was wrong — the reviewer whose SSO
  wrote it, or the team that deployed the tool? If the answer to the second is "the reviewer," they will figure that out
  within a month and either quietly stop using it or start approving without reading, and both outcomes are worse than
  not shipping. There's also a slower cost: investigators learn root cause analysis by struggling through it, and if the
  junior cohort never struggles, in five years you have a department that can approve investigations and can't conduct
  one.

  ═══════════════════════════════════════════════════════════════════
                           THE VERDICT
  ═══════════════════════════════════════════════════════════════════

  POSITION: Build it, but strip write credentials from the MCP server entirely — Claude proposes a hash-locked payload,
  a separate validated reviewer app commits it under the user's SSO, and you start with quality incidents and read-only
  investigation critique, not CAPA closure records.

  CONFIDENCE: 68% — The architecture is sound and the regulatory path through GAMP 5 / CSA is real if the qualification
  boundary sits at the human approval gate; what would move this to 85% is a seeded-error study showing reviewers catch
  >90% of defective drafts, and what would drop it below 50% is QA declining to sign an intended-use statement that
  classifies model output as unverified draft data.

  ──────────────────────────────────────────────────────────────────

  CRITICAL RISKS (exactly 3)

  1. False Attribution Trap: Your strongest claimed advantage inverts under inspection — the audit trail asserts a named
  human authored content Claude generated, which is a direct ALCOA attributability failure and reads as institutionalized data-integrity misstatement rather than traceability.
  2. Rubber-Stamp Collapse: Under any realistic volume the approval gate degrades into click-through, at which point every compliance control you built is nominal and the measured error rate of the whole system equals the model's unreviewed error rate.
  3. Unvalidated Custom Write Path: An MCP server that writes to GxP objects is a GAMP 5 Category 5 bespoke application, and yours currently permits non-atomic multi-object writes that orphan CAPA records, direct status__v writes that bypass workflow e-signature, and attachment text flowing toward write fields as a prompt-injection vector.

  ──────────────────────────────────────────────────────────────────

  NEXT STEPS (exactly 5, in order of priority)

  4. Disable the six write tools in the deployed MCP config today — deviation__v, capa_action__qdm, investigation__qdm, root_cause__qdm, capa_deviation__v, quality_incident__v — and keep the read/search surface. This is first because the current server can already write to a validated system without a validation package, which is the actual live exposure.
  5. Write a one-page intended-use and GAMP 5 risk-categorization statement with QA and CSV, explicitly declaring model output "unverified draft data" and the human approval gate as the qualification boundary — and get their signature or their objection before writing more code.
  6. Run the Scientist's two studies: 200 retrospective closed deviations blind-scored against approved finals, and a 20-in-100 seeded-error reviewer catch-rate test. Do not build the reviewer app until you know whether reviewers catch defects.
  7. Spike the proposal-and-commit architecture: hash-locked JSON payload, field-level diff UI, hash-equality assertion at commit, compensating rollback for junction writes, hard block on status__v and on any attachment-derived value reaching a write field, plus custom provenance fields on each object (AI-assisted flag, model ID, payload hash, reviewer edit count).
  8. Open the vendor conversation with Veeva on supported AI write provenance and native e-signature-compatible AI drafting, so the validation burden migrates to their quality system rather than compounding in yours.

  ──────────────────────────────────────────────────────────────────

  MINORITY REPORT: 🎨 THE VISIONARY

  "You're about to spend nine months and a validation package to win the right to type faster into a database. Point it backwards instead — a read-only critic that red-teams human-written investigations against the SOP and the last five similar events captures most of the value, needs no Part 11 surface, no reviewer app, and no signature from QA. Nobody's doing it that way because everyone assumes the bottleneck is drafting. It isn't. It's investigation quality."

  ═══════════════════════════════════════════════════════════════════