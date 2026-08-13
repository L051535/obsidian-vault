---
doc_number: TL-340513
version: "4.0"
status: Effective
effective_date: 2025-10-24
training_id: GSOP-104-1-TL-1
tags:
  - nalo/sop
  - nalo/deviation-management
---

# Batch Bracketing

**Supersedes:** GSOP-104-1-TL-1 v3
**Associated Procedure:** [[GSOP-104-1 Record Creation and Fact Finding|GSOP-104-1]]

## Purpose

This global required tool describes the documentation process for batch information associated with deviations.

## Scope

- **In Scope:** All quality deviations originated within the electronic Global Deviation Database (GDD).
- **Out of Scope:** Health, Safety, Environmental deviations or Laboratory Investigations.

## 1.0 Overview

All deviations must be assessed for involvement of/impact to batches at the time of, or as a result of, the deviation event. Batches must be included in the deviation record with initial impact documented for each. At the conclusion of the investigation, a final impact conclusion must be applied to each batch — this final impact is a *recommendation* for disposition; batch release personnel make the actual release decision, weighing the final impact plus all other relevant information.

## 2.0 Determine Involvement/Impact; Add Batches and Batch Bracketing Rationale to Deviation Records

| Step | Activity | Related Information | Responsible |
|---|---|---|---|
| 1 | Identify Batch Involvement | Select Batch Involved at Record Creation or Fact Finding. "N/A" is only available for Function/Non-Manufacturing areas that don't routinely process batches (no rationale required if N/A). | Initiator, Fact Finder |
| 2 | Document the Batch Bracketing Rationale | Include all involved/impacted batches in the Batch Bracketing Data Source & Rationale description, with a reference to source data. Justify the rationale, or why no batch bracketing is required. | Initiator, Fact Finder |
| 3 | Add Batches to the Deviation Record during Fact Finding | Add involved/impacted batches within 3 calendar days of record creation. Batches identified later must be added and assessed ASAP, with delay rationale documented. | Initiator, Fact Finder |
| 4 | Add Batches to the Deviation Record during READY Assessment | Condition: batches identified during READY Assessment. Complete the verdict as "Updates Required" and send back to Fact Finding — the impacted-batch list cannot be changed during READY Assessment itself. Delay rationale must be documented. | READY Assessor |
| 5 | Add Batches Identified During Investigation | Condition: batches identified during investigation. Mark "Yes" on Additional Batch(es) Involved in the Investigation Details and Summary section, and provide rationale/source data. Document initial batch impact; collaborate with the READY Assessor to lock it (required to enable the Final Impact field). See GSOP-104-3 (Investigation). | Investigator, READY Assessor |
| 6 | Remove a Batch from the Record at Fact Finding | Condition: a batch was added in error. Can be deleted during Fact Finding; batches that pass READY Assessment become system-locked and cannot be removed. | Initiator, Fact Finder |
| 7 | Remove a Batch from the Record During Investigation | Condition: a batch was added in error. Removable up until the READY Assessor assesses/locks it. If it can't be removed, its final batch impact must still be assessed per Section 4.0 (Out of Scope). | READY Assessor, Investigator |
| 8 | Use of Placeholder Batch for Contract Manufacturing Organizations (CMO) | Condition: the Lilly batch number isn't known yet, or the batch isn't a Lilly batch. Use "CMO – Lilly Batch Pending" (Lilly product, batch number not yet known — must switch to the real number once known) or "CMO – No Lilly Batch" (intermediate product not supplied directly to Lilly, or number unavailable before deviation approval). | Initiator, Fact Finder |
| 9 | Removal of Placeholder Batch | Once the real Lilly batch number is known (before the due date), replace the placeholder with it before routing for review/approval. If still unknown by the due date, replace "CMO – Lilly Batch Pending" with "CMO – No Lilly Batch." Removal can happen at Fact Finding or during Investigation, but not at READY Assessment; no other batch type can be removed while a deviation is in investigation. | Initiator, Fact Finder, Investigator |

## 3.0 Initial Batch Impact

| Step | Activity | Related Information | Responsible |
|---|---|---|---|
| 1 | Add the Initial Impact to Batch(es) | Add and select initial impact for each batch. Note: SAP and GDD are interfaced, but setting an initial impact in GDD does not change SAP status — it only flags SAP that a deviation is impacting the batch; SAP status updates are manual. | Initiator, Fact Finder, READY Assessor, Investigator |
| 2 | Radiopharma Release Selection for Initial Batch Impact | Condition: "Radiopharma Release" selected. Create a User Task for the Batch Release QA Manager/Qualified Person to document that the batch may be released prior to deviation closure; once assessed, personnel with product oversight must lock the final batch impact. | Initiator, Fact Finder, READY Assessor |
| 3 | Bulk Batch Disposition | Condition: all batches need the same initial impact. Use Bulk Batch Disposition — doesn't affect batches that already have an initial impact selected (update those manually). | READY Assessor |
| 4 | Complete the Assessment | State auto-changes to "assessed" once the record passes READY Assessment; must be completed manually for batches added at Investigation. Locked once past READY Assessment or once the initial batch workflow completes. | READY Assessor |
| 5 | Create a Risk Assessment for Released Product | Condition: involved/impacted batches were already released (incl. to other Lilly sites). Create a User Task for the Risk Assessor (e.g., TS/MS); complete immediately — recommended within 24 hours of discovery. See GSOP-104-1-TL-6 (Product Risk Assessment). | — |

**Initial Impact Status options:**

| Status | Description |
|---|---|
| Hold | Involved/potentially impacted but doesn't need re-inspection (e.g., already released and within Lilly/CMO/Distributor control). |
| Quarantine | Impacted/potentially impacted, at risk of rejection — must be quarantined, not released without (re-)inspection. |
| Reject to Scrap | Impacted/potentially impacted and rejection is confirmed — shall not be released/distributed. |
| Forward Processing Prior to Release | Impacted/potentially impacted; may be forward processed but not released/distributed pending deviation approval and next steps. |
| Radiopharma Release (short shelf life) | Radiopharma batches only — impacted/potentially impacted but acceptable for release based on preliminary assessment plus short shelf life. |

## 4.0 Final Batch Impact

| Step | Activity | Related Information | Responsible |
|---|---|---|---|
| 1 | Apply Final Batch Impact | Add final impact to each batch before concluding the investigation — individually, or via Final Bulk Disposition (batches with a final impact already set before bulk disposition must be updated manually if needed). | Lead Investigator, READY Assessor |
| 2 | Deviation Approval and Batch Impact | Approving the deviation confirms the selected final batch impact status; the approval must include an approver with product quality oversight. | Approver(s) |

**Final Impact Status options:**

| Status | Description |
|---|---|
| Rework/Reprocess | Confirmed impacted — requires rework/reprocessing/reidentification; shall not be released/distributed. |
| Reject to Scrap | Confirmed impacted — full rejection (not released/distributed) or partial rejection (rejected portion documented in Investigation Details, remainder released). |
| Reject to Return to Vendor | Confirmed impacted — requires rejection and return to vendor. |
| Approve | Confirmed to meet market/Clinical Trial release or distribution requirements — shall be released/distributed. |
| Approved (Limited) | Limited to a specific country/region, formulation, or reserved for a specific business activity (e.g., validation, non-GMP development); includes release under concession with justification in Investigation Details. |
| Out of Scope | Confirmed not involved in the deviation's impact — shall be released/distributed. Requires a User Task (Batch Impact Assessment) assigned to the READY Assessor, with QA M3 or Product Quality Leader/IBUQA P4 confirming alignment before release/distribution can precede deviation approval. |
| CMO Disposition Confirmed | A CMO-recommended disposition that Lilly has agreed to; documented in the deviation record. |

> [!note]
> Only personnel with product oversight responsibilities may confirm final batch impact; confirmation occurs at record approval.

---
*Source: GSOP-104-1-TL-1, Version 4.0, Effective 24 Oct 2025. Approved 24 Oct 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
