---
doc_number: PRD-86821
version: "25.0"
status: Effective
effective_date: 2025-08-19
training_id: USD-SOP-Stock Withdrawal-001
tags:
  - nalo/sop
  - nalo/stock-withdrawal
---

# Stock Withdrawal of Finished Products

**Supersedes:** 001-001113, 001-001086, 006680
**Areas Involved:** North American Logistics Operations (NALO) Operations, North American Shared Service Center – Customer Service and Inventory Management (NASSC-CS), NALO Quality Assurance

## Purpose

Describes requirements for ad-hoc stock withdrawal of finished products after it has been sent to US or Canadian Affiliate distribution operations inventory — covering the request, documentation, and system transactions for withdrawals within the United States, Canada, and Puerto Rico.

## References

- CS-SOP-13892, Handling and Completion of DEA Form 222s by Purchasers and Suppliers (US)
- USD-SOP-FDA Product Sampling-001
- USD-SOP-Processing Orders Governed by Registry-077
- USD-SOP-NASSC_CS Sales Order for Trade Product Entry-012
- USD-SOP-Phys Trnfr Form-001
- Global Records Retention Schedule
- CS-SOP-002, Transfer of Controlled Substances (US)
- CS-FRM-002-01, Controlled Substance Transfer Form (CSTF)
- CS-JOB-002-03, Verification of DEA Registrations for Controlled Substance Transfers

## Introduction

Requests to withdraw finished stock come from many areas of the company or third parties, covering any distribution center owned by Lilly or contracted by a Lilly Affiliate to distribute finished stock to first paying customers. Third parties include partner companies that own the stock in a U.S. or Canadian distribution — no costing is associated with the material or its withdrawal.

**Process:**
1. Requestor submits a written request to NASSC-CS via **Attachment A** (NALO Finished Stock Withdrawal Form) or **Attachment B** (NALO Finished Stock Withdrawal Form-Flip Logic Clinical Trials), signed/dated by the Requestor AND Requestor's supervision or budget owner.
2. Written approval from Management in NALO / NALO QA is required unless otherwise noted.
3. Return instructions are provided on Attachment A or B.

> [!note]
> Attachments A/B and associated documentation are managed per the Global Records Retention Schedule.

**Exceptions to this process:**
- **FDA Request** (agency pulls samples on-site): follow USD-SOP-FDA Product Sampling-001 instead.
- **Clinical Trial Material**: alternate form/process direct from third-party requestor.
- **Lilly Pharmacy**: alternate form/process.

> [!warning]
> CS product(s) may **NOT** be sent via interoffice mail or regular mail.

PDC Operations follows USD-SOP-Phys Trnfr Form-001 for local product transfers, if needed.

## Additional Approvals

**If request is for a Controlled Substance (CS):**
- Schedule II requests require a DEA Form 222 attached to Attachment A/B (contact GQAAC for a blank form).
- All CS transfers require a completed Controlled Substance Transfer Form (CSTF) per CS-SOP-002, unless all required info is already on the packlist.
  - Non-Lilly requestor → consult the CS Officer on documentation requirements.
  - Lilly employee/contractor → check the Controlled Substance Authorization List to confirm authorization.
  - Third-Party company → check Quality/contractual agreement for CS requirements before shipping to Third Party Operations (TPO).
- Sales Representatives are **not** allowed to receive Controlled Substances.

**If request is for a Promotional Sample** not going to a sales rep or licensed prescriber: written approval needed from Consumer Product Quality Assurance / Sample Operations. (If the sample is also a CS item, CS steps apply too.)

## Entering Product Requests into SAP

NASSC-CS enters an order in SAP (transaction `VA01 – Create Order`) per these steps:

1. Review Attachment A/B for completeness; confirm NALO inventory management + NALO management approval. If CS is included, see "Additional Requirements if the Request Includes a Controlled Substance" below.
2. Start order creation: `Logistics > Sales and Distribution > Sales > Order > VA01`.
3. **Determine Order Type:**

| Region / Charge | Order Type |
|---|---|
| US/PR, requestor WILL be charged, Clinical Trial (Trial Alias contains xxx-MC-xxx) | Z14 |
| US/PR, requestor WILL be charged, Clinical Trial (other alias) | Z12 |
| US/PR, requestor WILL be charged, NOT Clinical Trial | Z14 |
| US/PR, requestor will NOT be charged | Z10 (also used when shipping to a partner/third party who already owns the stock) |
| Canada, Clinical Trial | Z12 |
| Canada, not Clinical Trial, free of charge, no medical ingredient | Z10 |
| Canada, not charged, intended to generate revenue, not full treatment period (samples) | Z11 (cost center overwritable at order entry) |
| Canada, not charged, not intended to generate revenue, can be full treatment period (patient can't afford) | Z13 |

4. **Sales Org / Distribution Channel / Division:**

| Scenario | Sales Org | Dist. Channel | Division |
|---|---|---|---|
| US Affiliate trade or sample | 1100 | 01 | 01 (Pharma) |
| Puerto Rico trade | 1501 | 01 | 01 (Pharma) |
| Puerto Rico sample | 1100 | 03 | 01 (Pharma) |
| Canadian | 1429 | 01 | 01 (Pharma) |

5. Confirm to standard order entry screen (same for all order types).
6. Enter Purchase Order number (US Clinical Trial → Trial Alias, e.g. `B5K-MC-IBDS`; else Requestor's name unless form specifies otherwise).
7. Enter Sold To party:

| Region | Sold To |
|---|---|
| US, Clinical Trial | 191769 - CLINICAL FIELD TRIALS - LILLY |
| US, other | Requestor's Sold To SAP Customer Number, else One Time Customer 56000089 |
| PR | Requestor's Sold To number, else contact BPKC for One Time Customer Number |
| Canada, Clinical Trial | 56000045 - CLINICAL FIELD TRIALS - LILLY |
| Canada, other | Requestor's Sold To number, else One Time Customer 56000043 |

8. Ship To defaults from Sold To. US Clinical Trial Ship To is typically `427250 - FISHER CLINICAL SERVICES`. If Ship To doesn't match the order form, contact the Customer Master data steward.
9. `GOTO > HEADER > ACCOUNT ASSIGNMENT` — enter accounting code:

| Requestor | Account Assignment / Cost Center |
|---|---|
| Lilly employee outside US Affiliate, Clinical Trial (Trial Alias xxx-MC-xxx) | 100ACTP |
| Lilly employee outside US Affiliate, Clinical Trial (other) | Auto-determined in SAP |
| Lilly employee outside US Affiliate, not Clinical Trial | Cost center from request form |
| US Affiliate (e.g. Brand Team) | Listed on the Stock Withdrawal form |
| US Sales Force | 100A042 |

10. Enter Requested Delivery Date if needed. Saturday deliveries require notifying Operations/NALO Logistics plus a recipient contact name/phone.
11. Check/modify Shipping Route as needed (see NASSC Customer Service Collaboration site for common routes). Email outside warehouse (e.g. Canadian UPS warehouse) if route changes on an electronically-fed order. Internal Local Lilly employee shipments: Shipping Condition = Emergency, Route = USLOCL. CS shipped internally cannot be delivered weekends/holidays — CS Authorized personnel must be available to receive and complete the CSTF.
12. Enter Shipping Condition if needed (`Goto > Header > Shipping > Shipping Conditions`) — 'Emergency' for US/PR, 'Express' for CA when overriding automatic route determination.
13. Enter Item Code in Material field, press Tab.
14. Enter batch/serialized info if specific batches or serialized units required.
15. Enter quantity in Order Quantity field, press Enter.
16. Repeat steps 14–17 until order is complete.
17. Complete the **NALO Customer Service Use Only** portion of Attachment A/B.
18. Save supporting documentation.

> [!warning]
> CS orders **MUST** have a .pdf or screenshot of the DEA website showing Ship To's validated DEA registration attached to the SAP sales order. Send a copy to the CS officer for filing if a CSTF was completed (not required if all packlist info is present).

| Documentation type | Action |
|---|---|
| Hard copy | File in appropriate departmental file per Global Records Retention Schedule |
| Electronic copy | In SAP, open order in change mode (VA02) → Systems → Services for Object → Create Attachment → select file → Open |

## Additional Requirements if Request Includes a Controlled Substance (US Only)

| Step | Action |
|---|---|
| 1 | Lilly employee requests CS: verify DEA registration via DEA website or GQAAC sharepoint; check Controlled Substance Authorization List. |
| 2 | Third Party/External Owner requests CS: contact External Manufacturing/Alliance Management to confirm agreement includes CS requirements, receiver has current unexpired DEA registration matching Ship To address and covering the requested Schedule. Obtain DEA Registration Number and signature/approval documentation. |
| 3 | Schedule II CS: obtain DEA Form 222 before shipping (ref. CS-SOP-13893). Validate Receiver's DEA license (address match, not expired, correct Schedule). Attach DEA website screenshot to SAP sales order. Obtain CSTF 002-01. Provide both forms to NALO Operations/third-party distribution. Send DEA Form 222 copy to GQAAC for ARCOS reporting. |
| 4 | Return to Entering Product Requests into SAP section. |

---
*Source: USD-SOP-Stock Withdrawal-001, Version 25.0, Effective 19 Aug 2025. Approved 31 Jul 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
