---
doc_number: PRD-86725
version: "15.0"
status: Effective
effective_date: 2024-04-01
training_id: USD-SOP-FDA Product Sampling-001
tags:
  - nalo/sop
  - nalo/fda-sampling
---

# Regulatory Product Sampling in US Finished Goods Distribution

**Supersedes:** 001-004545, 001-001974-002, Lilly Procedure 017001-004
**Areas Involved:** North American Logistics Operations (NALO) Operations, NALO Quality Assurance

## Purpose

To provide a process for when a regulatory agency requires samples from US Finished Goods Distribution — escorting the agency, obtaining the sample, providing Lilly duplicate samples for examination/testing if applicable, and preparing the event summary report.

## References

- USD-SOP-Stock Withdrawal-001
- USD-SOP-Regulatory Interactions-001
- Global Records Retention Schedule
- CS-SOP13892, Handling and Completion of DEA Form 222s by Purchasers and Suppliers (US)
- Lilly Quality Standard 112, Regulatory Authority Communications and Inspections
- Global Quality Standard 101, Quality Management
- LQP-101-1, Notification to Management (NTM)

## Reasons for Revision

Replaced the reference to SSS Procedure 13893 (retired) with CS-SOP13892, Handling and Completion of DEA Form 222s by Purchasers and Suppliers (US) — content moved there.

## Introduction

NALO Operations/QA's role depends on the sampling type. If sampling product **already in commerce**, the event is owned by IDM, IDAP, or EMSA as appropriate. If sampling product **not yet released by the FDA for commerce** (e.g., from Fegersheim or Sesto), the process is owned by NALO QA.

## If the Sampling Is of Product Already in Commerce

### Roles and Responsibilities

| Role | Responsibility |
|---|---|
| NALO Quality Assurance | Contacted by Site Quality Assurance (SQA) for the appropriate plant (IDM, IDAP, EMSA) to check product availability. Ensures adequate samples exist for the agency's request plus Lilly duplicate samples — for Plainfield, by running the SAP MB52 report per requested Item Code (Regional QA or designee does this for Regional Distribution Centers). |
| NALO Operations | Escorts the regulatory inspector at all times during the visit. Pulls samples together with the NALO QA Representative (or designee at Regional centers). |

### Preparation for the Sampling Event

| Step | Action |
|---|---|
| 1 | If advance notice was given, go to "Information Supplied by the Regulatory Agency" below. If not, upon investigator arrival NALO QA (or designee at Enfield/Fresno) requests credentials, records the credential info, requests the nature of business on site, requests/reviews FDA Form 482 or other Sample Request paperwork, and documents all of it per USD-SOP-Regulatory Interactions-001. |
| 2 | Notify Management per LQP-101-1 (Notification to Management). |

### Information Supplied by the Regulatory Agency

On advance notification (received by Corporate/Site QA for the plant, or the Regional Distribution Center), the agency may supply: visit date; investigator/inspector names; reason for sampling; and items/lot numbers plus quantities requested.

> [!note]
> If a Regional Distribution Center receives the advance notification, it must immediately inform the appropriate plant's SQA Representative, who then communicates to the proper personnel.

### Establishing How and Where the Regulatory Agency Samples

1. The plant's SQA establishes how/where sampling will occur (using the item code/batch, quantity requested, and reason for sampling to determine sample source).
2. SQA contacts NALO QA to check availability — NALO QA runs the SAP MB52 report per item code (Plainfield QA rep for Plainfield; Regional QA rep or designee for Enfield/Fresno).
3. Agency samples plus Lilly duplicates are taken from the Distribution Center, from approved in-commerce material.
4. If material isn't available at the Distribution Center, SQA informs the agency; if samples must still be supplied, they may come from the House Samples area (for drug products, see "Review of Sample Availability and Copies of Methods" below, after review with the Global Quality Leader for the Lilly Technical Center).
5. Finished-product samples must be fully finished (labeled/packaged as for market) — nude vials/bottles must never be supplied.

### Review of Sample Availability and Copies of Methods

1. Plant SQA reviews sample availability to ensure enough exists for both the agency's request and Lilly duplicates. (The decision to analyze duplicate samples rests with the Global Quality Leader or equivalent.)
2. Quantity needed is based on potential testing — Manufacturing QA determines the quantity and prepares copies of Analytical Methods typically requested for non-compendial testing.
3. NALO QA (or designee at Enfield/Fresno) checks availability; if insufficient at the Distribution Center, samples may come from the House sample area, with SQA responsible for those samples.
4. If the agency wants to observe sampling and sufficient quantity exists at the Distribution Center, SQA informs NALO QA (or designee) — samples must **not** be pulled in advance in this case.

### Conducting the Sampling Event

SQA, NALO QA, and NALO center personnel will:

1. Meet and remain present with agency investigators at all times.
2. Review investigators' credentials and Notice of Inspection (FDA Form 482) or other Sample Request paperwork. (FDA Form 482 indicates the inspection reason is to sample Lilly products.)
3. Supply the requested samples or escort the agency to the pull area. *Regulatory agency personnel may never perform the sampling themselves — only authorized Lilly personnel (with PPE if applicable) may pull samples.*
4. At Plainfield, one individual each from NALO Operations and NALO QA must be present; at Enfield/Fresno, at least 2 Operators (one Operator + one Supervisor, or one Operator + one QA). Operations or QA should have a printout of the requested product's locations.
5. NALO Operations retrieves pallets, opens cases, and removes product, pulling duplicate samples for the Lilly lab. For non-serialized product, refill cases so only one partial case remains (marked "partial" with quantity, initials, date). For serialized product, don't refill partial cases — record serial numbers on the stock withdrawal forms instead. Every opened case is taped closed, initialed, and dated by Supervision/QA/Operations. *Any serialized-inventory adjustment requires a matching adjustment in ATTP — contact the Inventory Specialist and use transaction ZS518 (Serial Number Management).*
6. Ensure agency samples stay in original, approved container/closure systems with tamper-evident features and proper labeling — any exception needs site quality management approval/escalation.
7. Ensure samples (agency's and Lilly's duplicates) are stored per label requirements once collected.
8. Pack for transport to the agency/Lilly lab per local shipping procedures.
9. NALO QA (or designee) supplies with each sample: a Safety Data Sheet copy, the cost per unit (from the NALO Customer Services Account Representative), and non-compendial analytical method copies (from the plant).
10. NASSC-CS enters orders in SAP per USD-SOP-Stock Withdrawal-001, charging the correct cost center, and includes serial numbers for serialized product. *Two copies of Attachment A (Finished Product Request Form, from USD-SOP-Stock Withdrawal-001) are required per item — one for the agency's product, one for the Lilly-lab duplicate — noting unit cost and serial numbers.*
11. Prepare and provide a sample receipt (time, date, location, item code/title/batch, sample amount, other pertinent info) — FDA Form 484 may serve this purpose. *NALO personnel must not sign any other paperwork/reports beyond the sample receipt.*

### Additional Guidelines

- The regulatory agency does **not** have authority to: (a) sample DEA-controlled-substance items without a DEA representative present plus Sample Request paperwork; or (b) take photographs during sampling — if an inspector insists, Lilly must take duplicate photos for company use, and any insistence on video/audio recording should go to the Global Quality Leader or Corporate Legal before proceeding.
- Duplicate samples (identical quantity/location/lot, pulled at the same time as the agency's) are always taken. Store per label conditions before lab shipment. SQA must notify the responsible Manufacturing/Materials Center QA personnel that samples are pulled and awaiting shipment/testing (consult the plant's QA if the event occurred at a Regional Distribution Center).
- For DEA-controlled-substance items, follow CS-SOP13892 and USD-SOP-Stock Withdrawal-001 for withdrawal/documentation.

### Sample Analysis

The decision to analyze the sample rests with the Global Quality Leader. Duplicate samples are packaged/stored per conditions appropriate to the sample type.

## If the Sampling Is of Product Not Yet Released by the FDA for Commerce

*(Owned by NALO QA.)*

> [!note]
> The regulatory agency does not have authority to sample DEA-controlled-substance items without a DEA representative present plus Sample Request paperwork.

### Roles and Responsibilities

| Role | Responsibility |
|---|---|
| NALO Quality Assurance (Plainfield and Enfield only) | Ensures all pallets received on the requested Air Way Bills (AWB) are on the floor, grouped by item/batch. Copies corresponding TempTale files if appropriate. Documents the sampling event. Adds/ensures the event is logged in the Trackwise Audit Module. |
| NALO Operations (Plainfield and Enfield only) | Groups the requested product on the floor by item/batch. Escorts the inspector at all times. Pulls samples with NALO QA (or another designated Operations person if QA isn't on site). |

### Preparation for the Sampling Event

| Step | Action |
|---|---|
| 1 | Before investigator arrival, NALO QA Management notifies management per LQP-101-1. |
| 2 | Before investigator arrival, NALO Operations groups the requested product by item/batch (FDA typically requests by AWB number/import entry number). |
| 3 | NALO QA (or Operations) second-person-verifies the grouping. |
| 4 | On arrival: request/record credentials, request nature of business on site, request/review FDA Form 482 or other Sample Request paperwork, and document per USD-SOP-Regulatory Interactions-001. |

### Conducting the Sampling Event

NALO QA and NALO Enfield/Plainfield personnel will:

1. Meet and stay present with investigators at all times.
2. Review credentials and the Notice of Inspection (FDA Form 482 or equivalent).
3. Escort the agency to the sample-pull area. *Only authorized Lilly personnel — never agency personnel — may pull samples.*
4. One individual each from NALO Operations and NALO QA must be present; if QA isn't on site, 2 Operations personnel must be present.
5. Open cases and remove product; refill non-serialized partial cases to one remaining partial (marked "partial" with quantity/initials/date). For serialized product, don't refill — record serial numbers so inventory downgrades can update them too. Tape, initial, and date each opened case.
6. Ensure samples remain in original, approved container/closure systems with tamper-evident features/labeling — exceptions need site quality management approval/escalation.
7. Prepare/provide a sample receipt (time/date, location, item code/batch, amount, other pertinent info).
8. Ensure samples are stored per label requirements once in agency possession; supply package inserts/case labels/TempTale files/product labeling copies as appropriate.
9. Pack for transport per local shipping procedures, if appropriate.
10. NALO Operations/QA downgrades inventory for the sampled item/batch — for serialized samples, contact the Inventory Specialist to reconcile ATTP via transaction ZS518.

### Documentation of the Sampling Event

NALO QA (or designated Operations personnel, if QA isn't on site at Enfield) documents: names of FDA/DEA/Lilly personnel involved; dates/times/location; reason and sampling method; documents issued/signed (FDA Forms 482/484 or equivalent); product info (item code(s)/batch(es)/quantities sampled); quantity of duplicate samples pulled (for in-commerce product); who received the sample (name/agency); acknowledgment of receipt; and any other pertinent information/actions taken.

File a copy in the NALO GMP Library per the Global Records Retention Schedule, and ensure any sampling event for not-yet-released product is added to Trackwise 138.

### Record Retention

NALO Operations/QA retains in the NALO GMP Library (per the Global Records Retention Schedule): FDA Form 482 or equivalent; signed FDA Form 484 or prepared sample receipt; signed Notification to Management (from plant SQA or NALO Quality Management); the sampling-event documentation above; completed/signed Finished Product Request Forms per item sampled (USD-SOP-Stock Withdrawal-001, Attachment A); the MB52 report per item code, if applicable; and any other pertinent documentation (e.g., Notice of FDA Action for each AWB/import entry approved for release).

---
*Source: USD-SOP-FDA Product Sampling-001, Version 15.0, Effective 01 Apr 2024. Approved 12 Feb 2024. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
