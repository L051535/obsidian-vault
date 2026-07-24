---
name: update_trainings
description: Re-sync the converted markdown notes in "Resources/NALO Trainings MD/" against their source PDFs in "Uploaded Files/NALO Trainings PDF/". Use when the user runs /update_trainings, or asks to refresh/update the NALO training markdown notes after a PDF has changed (new revision, new file added, or a file removed).
---

# Update NALO Trainings

Keeps the markdown notes in `Resources/NALO Trainings MD/` in sync with the source PDFs in `Uploaded Files/NALO Trainings PDF/`. Each markdown note is a structured, hand-converted summary of one PDF (frontmatter with doc number/version/effective date, then sectioned content) — this skill re-derives that summary whenever the source PDF changes, and keeps the two folders 1:1.

## Folder layout

- Source PDFs: `Uploaded Files/NALO Trainings PDF/*.pdf`
- Converted notes: `Resources/NALO Trainings MD/*.md` (same base filename as the PDF, `.md` extension)

## Frontmatter standard

Every converted note's frontmatter must contain **exactly** these six fields, in this order — no more, no less:

```yaml
---
doc_number: PRD-XXXXX
version: "14.0"
status: Effective
effective_date: 2024-11-27
training_id: USD-SOP-Pallet Config-001
tags:
  - nalo/sop
  - nalo/some-topic
---
```

- `doc_number` — the `PRD-XXXXX` id from the PDF header.
- `version` — quoted string, exactly as printed (e.g. `"14.0"`, or `"3.0 (v13)"` for a redline mid-review).
- `status` — exactly as printed (e.g. `Effective`, `Pending Effective`).
- `effective_date` — ISO `YYYY-MM-DD`.
- `training_id` — the document's own short ID (e.g. `USD-SOP-Pallet Config-001`, `GQS304`, `GSCSOP0125`). This was previously named `sop_id` — always use `training_id` now.
- `tags` — namespaced `nalo/...` tags matching the doc's topic. Reuse existing tag names (`nalo/sop`, `nalo/security`, `nalo/warehouse-capacity`, `nalo/gqs`, `nalo/distribution`, `nalo/tms`, `nalo/bsr`, `nalo/serialization`, `nalo/dscsa`, `nalo/pallet-config`, `nalo/stock-withdrawal`, `nalo/exacta`, `nalo/worldlink`, `redline`) where applicable rather than inventing near-duplicates.

Do **not** put `supersedes`, `associated_gqs`, `secondary_gqs`, `document_type`, or any other field in frontmatter — that information belongs in the note body instead (see below).

## Procedure

1. **Inventory both folders.** List `Uploaded Files/NALO Trainings PDF/*.pdf` and `Resources/NALO Trainings MD/*.md`. Match them by base filename (ignoring extension) to find:
   - **New PDFs** with no corresponding `.md` — need a brand-new conversion.
   - **Existing pairs** — need a staleness check (step 2).
   - **Orphaned `.md` files** with no corresponding PDF — flag to the user; don't delete without confirmation.

2. **Check staleness for existing pairs.** For each PDF/markdown pair, read the PDF's header fields (Version, Effective Date, Status — found in the "Number: PRD-XXXXX Version: X.0 Status: ... Effective Date: ..." line near the top of page 1) and compare against the markdown's frontmatter (`version`, `effective_date`, `status`). Also compare file modification time as a secondary signal (`ls -la` or stat), since a PDF can be re-exported with the same version if corrected.
   - If the PDF's version/effective date/status match the markdown frontmatter exactly, skip — already current.
   - If they differ, or the PDF's file mtime is newer than the markdown's, the note needs updating.

3. **Convert/update each PDF that needs it.** Read the full PDF with the Read tool (it handles native PDF text extraction). Rewrite the corresponding markdown note following the established format used in this vault's existing NALO training notes:
   - YAML frontmatter: exactly the six fields defined in "Frontmatter standard" above.
   - A top-level heading matching the document title.
   - Immediately under the heading, a body line for anything that used to live in frontmatter but doesn't anymore — e.g. `**Supersedes:** 001-001280`, `**Associated GQS:** GQS304 (secondary: GQS301)` — alongside the existing `**Areas Involved:**` line, only when the source PDF actually has that field.
   - Sectioned body mirroring the PDF's own section structure (Purpose, Scope, numbered procedure sections, tables for step/action or "if/then" content, etc.). Use Obsidian callouts (`> [!note]`, `> [!warning]`) for the PDF's own Note/Warning call-outs.
   - If the source is a **redline** (tracked-changes/markup document — title contains "Redline" or the PDF shows inserted/struck text), open with a "Summary of Changes" section listing what changed vs. the prior version, then the full current-version content — follow the pattern in `Redline; GQS304, Distribution of Finished Products.md` in this vault. Note that the redline/version-transition context goes in the H1 and a `> [!warning] This is a redline` callout, not in frontmatter.
   - Closing italic source line: `*Source: <doc id>, Version <X.0>, Effective <date>. Approved <date> by <approver>. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*`
   - Do not fabricate content — if a section is illegible or cut off in the PDF extraction, note that explicitly rather than guessing.

4. **Report results to the user.** Summarize: which notes were updated (old version → new version), which were newly created, which were already current, and any orphaned markdown files found with no matching PDF (ask before deleting those).

## Notes

- Never delete or overwrite a markdown note that has no PDF-driven reason to change — only touch files identified as stale or new in step 2.
- If a PDF's content spans many pages, read it in one pass — the Read tool handles PDFs up to 20 pages per request; split the request by page range for longer documents.
- Preserve any manual edits the user may have made to a note where possible — if the frontmatter shows the note is already current but the user explicitly says "force re-convert," regenerate anyway.
