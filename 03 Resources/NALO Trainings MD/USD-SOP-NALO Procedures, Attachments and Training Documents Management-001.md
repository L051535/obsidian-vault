---
doc_number: PRD-86950
version: "15.0"
status: Effective
effective_date: 2024-05-17
training_id: USD-SOP-NALO Procedures, Attachments and Training Documents Management-001
tags:
  - nalo/sop
  - nalo/document-management
---

# NALO Procedures, Attachments and Training Documents Management

**Supersedes:** 001-003984, 001-003986, 001-003987, 001-04546, USD-SOP-Regulus for Procedures and Training-001
**Areas Involved:** NALO Learning and Development (L&D), NALO Quality Assurance, and support areas

## Purpose

To provide instructions for processing Standard Operating Procedures (SOPs), associated Attachments, and Training documents in QualityDocs.

## References

- IT_GBL: QualityDocs Workflow Participant Training, Item# 9290122
- IT_GBL: QualityDocs Document Controller and Form Controller Training, Item# 9290123
- QualityDocs User Guide: Manage Document Change Log
- LQP-302-25, Risk Evaluation and Responsibilities for Computer Systems
- USD-SOP-NALO Training Proc-001
- GQS 105, Data Integrity and Good Documentation Practices
- Global Records Retention Schedule
- QualityDocs Performance Support Site

## Reasons for Revision

All revisions marked with an asterisk (*). Notable changes in this version's history: added Collaborative Workflows guidance (incl. QualityDocs Performance Support Site reference) to the Introduction; clarified that GQS/LQS content must not be copied/pasted directly into local procedures; updated the Forms definition (TR40634669); updated the Note(s) to File definition and added the Document Change Log user guide reference; required a Reasons-for-Revision statement to persist for procedures containing star (✻) symbols; added NALO Computer Systems Procedures to the Minimum Reviewers/Approvers table (per LQP-302-25); removed an obsolete note about QA providing effective dates for logbook-bound GMP forms (NALO no longer binds forms in logbooks); added recommended processing-time guidance to Submitting Documents for Revision (TR#40655343); added a note on Procedure/Attachment/Training Requests; clarified the Submitting Documents for Revision introduction; streamlined the Retirement Process introduction and reordered Step 2's bullets.

## Introduction

NALO Reviewers, Approvers, and the L&D Document Controller use this procedure together with IT_GBL Item# 9290122 (QualityDocs Workflow Participant Training) and Item# 9290123 (QualityDocs Document Controller and Form Controller Training) to process NALO procedures, attachments, and training documents in QualityDocs. Follow this procedure where it doesn't align with that training, and contact NALO L&D for clarification.

Collaborative workflows for NALO procedures/attachments/training documents in QualityDocs may be started by any NALO Document or Form Controller, but L&D must be informed of final versions so they can be routed for formal review/approval per this procedure (see the QualityDocs Performance Support Site for Collaborative Authoring info).

> [!warning]
> To ensure continuous alignment, NALO procedures should only *reference* GQS/LQS or other governing documents — never copy/paste their content directly in.

## Terms and Definitions

| Term | Definition |
|---|---|
| Approver | Performs "Approve"/"Reject" on documents for areas they're responsible for — confirming it's been reviewed by the right SMEs and adequately addresses the document's purpose/scope. |
| Annotations | Comments/markings made within the QualityDocs document viewer to give feedback during a Review workflow. |
| Attachments (for Procedures) | Support a procedure's content, residing within or outside it; can also be "Forms." Use only *effective* Attachments (verify Status = Effective in QualityDocs). No training is required on Attachments themselves. |
| Comments | Feedback made *outside* the document viewer during a Review workflow. |
| L&D Document Controller | A NALO L&D member with Document/Form Creator permissions who manages documents (incl. Executed Electronic Form document types) through QualityDocs workflows as workflow owner. |
| Forms | Documentation recording activity execution/data collection. Must be controlled if it records raw GMP data, indicates a GMP activity occurred, or documents a product-quality decision. Controlled, printable forms must use the "Master Executable Form" Document Type. NALO calls its procedure forms "Attachments." |
| Note(s) to File | Used to capture changes for future procedure/attachment/training revisions. Contact the L&D Document Controller to request a Document Change Log in QualityDocs (see the QualityDocs Performance Support Site). |
| QualityDocs | The Veeva-vault, cloud-based, company-wide controlled document management system. |
| Reviewer | The SME representing their area in reviewing Procedures, Attachments, and Training — responsible for annotating/commenting on drafts so the final document accurately, completely, and unambiguously describes how to perform the task correctly and consistently. |

## Periodic Review Frequency

| Document Type | Periodic Review Period |
|---|---|
| SOPs | 3 years |
| Attachments to SOPs | 3 years |
| Non-GMP SOPs | 0 years |
| Training Forms | 0 years |
| Training Material | 3 years |

> [!note]
> The source PDF's table had this column pairing slightly ambiguous in extraction ("0 years" appearing between SOPs and Attachments rows) — the pairing above reflects the most sensible reading of the source; verify against QualityDocs if precision matters.

Periodic review dates normally reset whenever a revised document becomes effective.

## Required Elements for Procedures

| Element | Location | Description |
|---|---|---|
| SOP Heading | Body | Centered on page 1: "STANDARD OPERATING PROCEDURE / ELI LILLY AND COMPANY / \<OWNING AREA OF PROCEDURE\>" (e.g., North American Logistics Operations). |
| Title / Name (formerly "Procedure number") | Body | The procedure's title. Documents migrated from Regulus keep the same name with the legacy unique number at the end (e.g., "-001") — that suffix is a Regulus-assigned identifier, not a version number. |
| Superseded procedure number(s) | Body | New QualityDocs procedures keep the naming convention (USD-SOP-Name of the Document) without the trailing "-###". Superseded procedures list their prior SOP number(s) (e.g., a NovaManage Procedure number) — always keep the prior NovaManage and/or Regulus number here, since it's needed during site audits. |
| Purpose | Body | A short description of the procedure's objective. |
| Areas Involved | Body | Areas with tasks/responsibilities in the procedure — specific enough to identify who the procedure applies to (e.g., site like NALO Operations, or functional unit like NALO QA / NALO Engineering and Maintenance). Training on the procedure is **required** for Areas Involved. |
| Areas Affected (if applicable) | Body | Areas impacted by the procedure's outcome, or responsible for the next process stage — listed separately from Areas Involved. Training is **optional**, and Areas Affected don't approve the procedure. |
| Approvals | Body | Lists required Approvers by position title. |
| References | Body | All documents referenced in the procedural description — may include governing documents (GQS/LQS, etc.). |
| Reasons for Revision (an "*" denotes changes) | Body | For revisions: clear, complete, accurate statements of what changed and why (with rationale). Referenced procedures only need their identifier (e.g., "USD-SOP-Site Quality Plans-001"). An asterisk (*) marks in-body changes — not needed if "revised in its entirety." Procedures containing ✻ symbols keep a persistent statement in this section. |
| Procedural Description | Body | The procedure's main body. *("Procedural Description" isn't a required literal heading.)* |
| Page number | Footer | "Page X of Y" format at the bottom of each page (e.g., "Page 4 of 6"). |
| Training Course Number and Item Number with Revision Number | Footer | The applicable training course/item/revision number — the footer's revision number is the *item's* revision number. |

## Minimum Reviewers and Approvers for Procedures and Attachments

*(See USD-SOP-NALO Training Proc-001 for training-document review/approval requirements.)*

| For | Reviewers | Approvers |
|---|---|---|
| GMP Procedures | 1 person per functional area (covering each Area Involved); a QA Reviewer | Management of each functional area (or least-common functional Management of the Areas Involved); Management, NALO QA |
| NALO Computer Systems Procedures and Attachments | 1 person per functional area; NALO Computer Systems QA Representative (CSQA) | Management of each functional area (or least-common functional Management); NALO CSQA; Management, NALO QA |
| GMP Attachment(s) | 1 person per functional area | Management of each functional area using the Attachment(s) (or least-common); NALO QA Representative |
| Non-GMP Procedures (not Health/Safety/Environmental) | 1 person per functional area | Management of each functional area (or least-common) |
| Health, Safety, Environmental Procedures and Attachments | 1 person per functional area; a Safety Representative | Management of each functional area (or least-common); a Safety Representative |

## Abbreviated Approval Criteria and Appropriate Use

If all changes fall within these criteria, **no Review step is required** — the L&D Document Controller documents in Reasons for Revision that the changes meet Abbreviated Approval criteria (e.g., "All reasons for revision meet the Abbreviated Approval Criteria"):

- Format/style change with no content or instruction-sequence change
- Adding/changing/removing a contact person
- Changing subject/title/purpose wording to more accurately reflect content
- Adding notes/clarifying statements/editorial changes that enhance instructions without changing their intent or how they're performed
- Adding/removing definitions *(changing a definition does NOT qualify)*
- Correcting obvious factual errors (typos, a referenced procedure's number/title, a nonexistent item code)
- Changing department/division names/numbers where the function stays the same

## Submitting Documents for Revision

Before submitting for revision/retirement, check whether other Procedures/Attachments/Training need revising or retiring too, and submit all related changes to the L&D Document Controller together. Documents are written in active voice with correct spelling/grammar.

> [!note]
> Recommended minimum processing times: Document control — 3 business days; Reviews — 5 business days; Approvals — 5 business days; Training — 13 business days before the effective date if retraining is required (3 days LMS processing + 10 days for learners to complete training).

> [!note]
> Procedure/Attachment/Training Requests are for L&D information only — not for GMP purposes. Only NALO L&D Document Controllers may route procedures/attachments/training documents for formal review and approval in QualityDocs.

| Scenario | Process |
|---|---|
| New procedures/attachments/training documents | Submitter enters a Procedure/Attachment/Training Request (with all required info) on the NALO L&D Collaboration site, then sends draft document(s) to the L&D Document Controller. L&D adds training info and follows the review/approval preparation steps below, then routes in QualityDocs. |
| Revised procedures/attachments/training documents | Submitter edits a copy in Microsoft Word with Track Changes on, then enters the Request and sends the edited draft(s) to L&D. L&D adds training info and follows the same review/approval preparation steps, then routes in QualityDocs. |
| Retiring procedures/attachments/training documents | Submitter enters the Request; L&D routes the document(s) per the Retiring section below. |

## Reviewing

### Guiding Documents for Review Consideration

Documentation hierarchy for review: **Lilly Quality Standards (LQS)** — fundamental quality/pharma quality objectives enterprise-wide, with accompanying **Lilly Quality Practices (LQP)** — direct how to implement LQSs → **Global Quality Standards (GQS)** — global policy derived from GMP laws/regulations (FDA, EMEA, WHO, CFR, ICH, Lilly registrations), each with a **GQS Reference Document** aligning global laws/regs to the GQS → some GQS have **Common Quality Practices (CQP)** (must be followed) and/or **Global Resource Documents** (optional implementation templates) → **Standard Operating Procedures (SOPs)** — written local practices/instructions for site processes, documentation, and record retention.

### L&D: Preparing Documents for Review

| Step | Action |
|---|---|
| 1 | Confirm a Procedure/Attachment/Training Request was submitted with all applicable info. |
| 2 | Check active voice, spelling/grammar, and that Track Changes was used (if applicable). |
| 3 | Check for duplicate info in other documents; check whether other procedures need revising; for new documents, confirm with the Submitter whether the content could instead fit an existing document. |
| 4 | Confirm all Required Elements are present, including footer course/item/revision numbers; Attachments must show the procedure's name and page count. |
| 5 | Find all tracked changes; confirm Reasons for Revision/Rationale are clear/accurate/complete. Mark limited in-body changes with an asterisk (*); for extensive changes, state "revised in its entirety" with rationale instead (no asterisks needed) — and if the revision responds to a change control/deviation/audit, note that plus "revised in its entirety." Any revision from an External Agency Commitment must be marked with a ✻ symbol so it isn't later deleted/altered without careful consideration — removing/editing such a change requires QA approval. |
| 6 | Before adding drafts to QualityDocs, verify: document name matches the title; Track Changes is off; page numbering meets format requirements; superseded procedure number(s) are noted; Reasons for Revision cover all changes with rationale; training course/item/revision numbers are accurate; References are internally consistent; final formatting is satisfactory. |
| 7 | Add the draft(s) to QualityDocs and submit for review. |

### Reviewers: Reviewing Process

Reviewers use Annotations in the QualityDocs viewer (multiple reviewers can annotate simultaneously and see each other's annotations in real time), per IT_GBL Item# 9290122. *Documents can't be checked out for editing during a Review workflow — contact the Document Controller for an editable copy if needed.*

| Step | Action |
|---|---|
| 1 | Evaluate the document against questions like: does it match what I'd expect for this activity? Is there a GQS describing minimum standards here? Is it accurate to what really happens? Is it internally and cross-document consistent? Do the Reasons for Revision (or, for training/attachments, the Workflow Details metadata) completely/clearly/accurately state what changed, why, and the justification? Does it clearly state WHEN steps happen and WHO performs them (by responsible area/title)? Are contact-area instructions clear about what happens when contacted? Are vague words ("appropriate," "applicable," "correct," "proper") avoidable, or would a minimally trained person understand them as written? Are "should/must/will/may/shall" used correctly (may/should = optional; must/will/shall = mandatory)? Are all If/Then branches addressed? Are referenced logs/checklists/forms formally approved? Are record retention requirements clear? Are review/approval responsibilities for cross-referenced documents clear? |
| 2 | Evaluate formatting: clarity/conciseness, accurate References section, free of typos/grammar errors, accurate headers/footers/page numbers. |
| 3 | **QA** assesses impact on other GMP documents, compliance with Lilly/Global Quality Standards and local procedures, regulatory/corporate requirements, correct governing-standard references, and any regulatory action/impact (notifying the L&D Document Controller if so). **Safety Representative(s)** assess potential safety impact (PPE, hazard communication, hazardous chemicals/equipment). |
| 4 | If no changes are needed, no Annotation is required; if changes are needed, add Annotations where needed. *Content questions shouldn't be left for the Document Controller — direct them to other reviewers (if not yet done) or the relevant process owner directly.* |

## Approving

*(For documents on a Retirement workflow, see Retiring below.)*

### L&D: Preparing Documents for Approval

After review, the L&D Document Controller incorporates Reviewer comments (working with SMEs as needed) — renewing the QualityDocs review if changes are significant — then sends the document(s) to the appropriate Approver(s) per the Minimum Reviewers and Approvers table. *When using a delegate Approver, note it in Workflow Details (e.g., "Jane Doe approving under Delegation of Authority for John Smith") with a link to the effective Delegation of Approval Authority.*

### Approvers: Approving Process

Approval confirms the document was prepared with the Approver's knowledge, that its purpose/scope is correct, and that it was tasked to the appropriate Reviewers/Approvers in QualityDocs. *(QualityDocs approvals run in parallel.)*

| Step | Action |
|---|---|
| 1–2 | View the Reasons for Revision (or, for training/attachments, the Workflow Details metadata) — evaluating document *content* is the Reviewer's job, not the Approver's. |
| 3 | From knowledge of the Reviewers representing your area(s): confirm they had the right skills/knowledge to judge whether the document unambiguously describes correct, consistent, standards-aligned steps, and that every Area Involved had a representative Reviewer. |
| 4 | Approve or Reject per IT_GBL Item# 9290122. *If any Approver rejects, the workflow returns to Draft, outstanding approver tasks are withdrawn, and the L&D Document Controller resolves issues before resending for approval (or sending back for review).* |

### L&D: After Approval

| Step | Action |
|---|---|
| 1 | Determine the effective date, considering: the original Request info; input from QA and affected-area personnel; the approval date for any related change control; and appropriate time to conduct training. |
| 2 | Notify Learning Services of the effective date. |

## Retiring

### Retirement Process

| Step | Actor | Action |
|---|---|---|
| 1 | L&D | Confirm a Procedure/Attachment/Training Request was submitted with all applicable info. |
| 2 | L&D | Enter the retirement reason in the document's Retirement Information metadata; start a Retirement workflow; choose Approvers per Step 3; choose the Retirement Approval Type. |
| 3 | L&D | Route on a Retirement workflow with Approvers: **GMP documents** — Management of the Area(s) Involved + Management, NALO QA. **Non-GMP documents** — Management of the Area(s) Involved. **HSE documents** — Management of the Area(s) Involved + a Health, Safety and Environmental representative. |
| 4 | Approvers | Review the Rationale for Retirement in the document's Retirement Information metadata. |
| 5 | Approvers | Consider: does the retirement reason apply to all possible Areas Involved? If the described activity is still needed, is there another document covering it for all Areas Involved? Should areas not on the Retirement workflow review it too? Are there other documents that should also be retired as a result? |
| 6 | Approvers | Approve/reject per IT_GBL Item# 9290122. |
| 7 | L&D | If approved: select a retirement date if necessary, and notify Learning Services if necessary. If rejected: the workflow ends immediately, the document stays Effective, and L&D contacts the document owner/initiator for resolution. |

## Distribution, Retention and Destruction

Training materials are distributed with enough lead time for adequate training before the effective date (e.g., 10 business days), unless Supervision/Management requests otherwise. Completed learning histories go to Learning Services for course credit, then to the GMP Library for retention per the Global Records Retention Schedule. Check QualityDocs before use to confirm printed materials are still effective, and destroy printed materials confidentially (shredding or a confidential bin).

---
*Source: USD-SOP-NALO Procedures, Attachments and Training Documents Management-001, Version 15.0, Effective 17 May 2024. Approved 29 Apr 2024. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
