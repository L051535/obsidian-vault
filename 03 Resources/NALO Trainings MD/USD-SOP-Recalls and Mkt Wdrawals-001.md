---
doc_number: PRD-86740
version: "19.0"
status: Effective
effective_date: 2025-02-28
training_id: USD-SOP-Recalls and Mkt Wdrawals-001
tags:
  - nalo/sop
  - nalo/recalls
---

# Recalls and Market Withdrawals in North American Logistics Operations

**Supersedes:** 001-004273, 001-001144-003
**Areas Involved:** North American Shared Service Center – Customer Service (NASSC-CS), Finished Good Returns, NALO Quality Assurance
**Areas Affected:** NALO Operations

## Purpose

To coordinate and implement recalls and market withdrawals of Pharmaceutical Drug Products in NALO.

## References

- Global Records Retention Schedule
- USD-SOP-Quarantine of Finished Products-001
- USD-SOP-Processing GMP Data Requests-001
- USD-SOP-Ship Recalled Mtl to 3rd Pty Vndr-001
- USD-SOP-Segregation Process and Scrapped Mtl Processing-001
- DO23676, NALO Good Documentation Practices and SPV Training
- Lilly Quality System (LQS) Glossary
- LQS111, Quality Auditing
- LQS135, Market Withdrawals
- LQS303, Controlled Drugs and Lilly-Designated Special Security Substances
- LQP101-1, Notification to Management
- GQS131, Recalls
- GQS132, Product Safety Assessments (PSA)
- SEQS 301, Pharmacovigilance
- CQP-131-1, Product Removal Forms
- GPR-501-5-115, Serialization Decommissioning Serialization Material; GPR-501-5-115-Tool 1

## Reasons for Revision

1. Added a note to the Introduction: process Attachment A (Market Action Response Plan Checklist) every time, regardless of a recall or mock recall. Rationale: TR40767349.
2. Removed Fresno references throughout. Rationale: CC40755631.

## Definitions

- **Effectiveness Check**, **Health Hazard Evaluation (HHE)**, **Market Withdrawal**, **Recall**, **Recall Classifications** — see the LQS Glossary.
- **Recall Strategy** — a planned course of action for conducting a specific Recall, addressing the depth of recall (which can be based on the Recall Classification), the need for public warning, and the extent of effectiveness checks.

## Introduction

Attachments (in the electronic document management system): **Attachment A** (Market Action Response Plan Checklist), **Attachment B** (Incorrectly Shipped Registry Governed Product Response Plan Checklist), **Attachment C** (Affiliate Recall-Mock Recall Information), **Attachment D** (Market Action Process Flow).

> [!warning]
> Process Attachment A every time — regardless of whether it's a recall or a mock recall.

Every NALO individual must immediately report information that could potentially result in a Recall or Market Withdrawal — including product-defect information from: deviations; stability issues; potential contamination; failure to meet specifications; labeling/label text issues; product complaints; adverse drug reactions/events tied to product quality; confirmed/verified tampering, counterfeiting, or falsification of Lilly product/packaging (including from within the legitimate supply chain or bearing a batch number valid for the material/dosage); audit results; biological product deviation reports (BPDRs); medical device reports (MDRs); medical devices posing unacceptable health risk (e.g., functional failures); or any other source (including sales/marketing personnel and contract manufacturers) questioning marketed product/device quality, fitness, or performance. Recalls/Market Withdrawals may also be initiated at a regulatory agency's request, and may trigger an internal review per LQS111.

This procedure covers NALO QA, NASSC-CS, and Finished Good Returns responsibilities. For overall recall responsibilities, see GQS131 (and follow CQP-131-1); for market withdrawal responsibilities, see LQS135.

Recall/Market Withdrawal activities must be initiated promptly and effectively at any time — sometimes before root cause and full defect extent are established, especially where a significant patient safety issue exists. NALO management must ensure NALO QA has sufficient staff to handle the market action with appropriate urgency.

Action effectiveness must be evaluated every 2 years — NALO QA meets this via the Self-Inspection process, ideally testing a distribution-side scenario. NALO also participates in GQAAC-led (or other) simulations (see CQP-131-1); mandatory GQAAC Simulated Market Action participation can count as that year's effectiveness check if written up as a Self-Inspection and approved, as can activities that also fed other reports (e.g., FDA 3-Day Field Alerts).

> [!note]
> Attachment C (required by EU GMP Guidelines 2013/343/EC §6.5) must be completed and sent to the Responsible Person, Eli Lilly Ireland Holdings Limited, every time a mock recall or recall is performed. EU mock recalls/recalls must occur and be documented at least every 2 years per GQS131.

For controlled drugs or Lilly-designated Special Security Substances, see LQS303.

GQAAC develops the Recall Strategy and documents action plans beyond Attachment A, based on the FDA-agreed recall classification; GQAAC sends all legally required FDA reporting per their own procedures' timing. State Boards of Pharmacy notifications, if required, are issued by the NALO QA Rep coordinating State Licensing. If the issue originated in distribution, NALO QA helps the recall coordinator draft other immediate reports (3-day field alerts, BPDRs, CIRM reports, etc.).

See Attachment D for a graphical illustration of how all groups work together during a market action.

## NALO QA Responsibilities During a Recall or Market Withdrawal

Document each step as it's carried out; retain per the Global Records Retention Schedule; make documentation available to Regulatory authorities on request.

> [!note]
> If the issue was discovered in NALO, follow all steps below (and CQP-131-1). If discovered elsewhere, follow all steps *except* #1, #3, and #11–13 (see Attachment A).

| Step | Action |
|---|---|
| 1 | Notify the site quality leader immediately. |
| 2 | Initiate the NALO Task Force: Finished Good Returns (and its 3rd-party service provider, potentially); NASSC-CS; Lilly Cares if affected (and its 3rd-party provider, potentially); Managed Healthcare Services (MHS); Enfield/PDC Operations Manager; Sample Accountability if affected; NALO Quality; GQAAC Recall Coordinators; a Supply Chain representative. |
| 3 | Immediately generate a Notification to Management per LQP101-1, if applicable. *Significant safety information on a Lilly product escalates per SEQS 301.* |
| 4 | Identify affected products/batches (scope). To find all in-date batches of a product: run BMBC with the Material field set to the number's first part + asterisk (e.g., "PU3104"), checking only "SLED outside remaining shelf life" on the Shelf Life Expiration Data tab. *For 1–2 batches, NALO QA can pull this; for more, the GQAAC Recall Coordinator obtains the Global Batch Traceability (GBT) Report from Supply Chain.* |
| 5 | Determine manufacturing/packaging sites, quantity manufactured, manufacture date, and pack size per affected batch (MMBE) — if MMBE shows product "in transit," contact NASSC-CS for the destination. *Same 1–2-batch vs. more-batches split as Step 4 applies.* |
| 6 | Run the Re-Id Report (ZS102) to check whether affected batches were re-identified. *Only needed if NALO QA is handling 1–2 batches directly — the GBT report already includes Re-IDs.* |
| 7 | If product is at either Distribution Center, quarantine/segregate it per USD-SOP-Quarantine of Finished Products-001 — physically segregated, sufficient capacity, clearly marked, secure, and access-restricted (CS/SSS need secure, limited-access storage; validated electronic segregation is acceptable in automated storage that can't be physically accessed without an electronic interface + security verification). Run the packed-not-shipped report and remove/quarantine anything packed (even on a truck); reconcile counts. *Quarantine may be delayed if the product must stay available in-market — it eventually still needs to happen.* |
| 8 | If product is at a Third Party Warehouse, notify them to segregate it. |
| 9 | Check the ZS178 ship history report or GBT report for Clinical Trials shipments; inform the Recall Coordinator of any found. |
| 10 | Notify Global Supply Chain (GSC) once product is blocked/ship-closed — GSC then runs the GBT report to locate shipped product by distributor/customer; QA/Materials/Operations (Business QA) verify content before any agency submission. NALO QA may be asked for customer totals/addresses/contact info (in and outside business hours), working with MHS if needed. *If withdrawal/quarantine isn't immediate, GSC may run GBT early for a market snapshot, then rerun it once product is quarantined.* |
| 11 | Review complaint records (including adverse events) for reports of similar defects. |
| 12 | Open a deviation; collect data for root cause investigation; identify preventive actions via Quality Risk Management principles. NALO QA completes a preliminary investigation report covering the checklist's required info; the final deviation investigation must complete within 30 calendar days of market action initiation. NALO QA may help investigate even without direct responsibility. See GQS131 for final-report approval/distribution requirements. |
| 13 | Initiate a Health Hazard Evaluation (HHE) per GQS132 if the action is due to a product quality defect. |
| 14 | Send remaining Enfield/Plainfield product to the Third Party Recall Service Provider when instructed, per USD-SOP-Ship Recalled Mtl to 3rd Pty Vndr-001 — or to destruction per USD-SOP-Segregation Process and Scrapped Mtl Processing-001 if GQAAC instructs. NALO QA ensures serial numbers are decommissioned before shipment (contact the NALO Inventory Control Specialist; see GPR-501-5-115 / Tool 1). |
| 15 | Complete Attachment A per DO23676 (NALO Good Documentation Practices and SPV Training). |
| 16 | Complete Attachment C and send to the Responsible Person, Eli Lilly Ireland Holdings Limited, every time a mock recall or recall occurs (EU: at least every 2 years, per GQS131). |
| 17 | File all investigation documentation in the NALO GMP Library per the Global Records Retention Schedule. |

> [!note]
> NALO QA may be involved in inspecting returned product — the Site Quality Leader, in consultation with GQAAC, evaluates whether/how much inspection is needed. Document inspection results (or, if not inspected, the rationale for skipping it) in the final report. Inspection is valuable for understanding the recall's cause but isn't always informative (e.g., when batch records/retention samples already confirm mislabeling).

## NALO Task Force Responsibilities for Recalls

*(Items are completed, initialed, and dated by Quality Assurance.)*

1. Early on, contact NALO Tech@Lilly to make them aware — they may be able to help.
2. Ensure "verbatim"/standby statements or core messages are received from the GQAAC Recall Coordinator (needed for AEs, external customers, and sales reps).
3. Give the verbatims to US NASSC-CS Management, who prep NASSC-CS reps for customer questions.
4. Ensure Finished Product Returns notifies third-party returns processors to contact the Lilly Recall/Withdrawal processor for info when asked.
5. If serial numbers of distributed recalled product are requested, contact the NALO Operations Inventory Control Specialist.

## NASSC-CS Responsibilities During a Recall or Market Withdrawal

1. Network with the wholesaler trade relations group (MHS) to inform customer-notification and reimbursement-method decisions.
2. Receive a copy of the official Recall letter (product recalled, where to send it, credit method — the same letter Lilly customers with affected product receive) plus FAQs from MHS, and be ready to answer customer questions about it.
3. Follow MHS's steps for product replacement, if applicable.

## Third Party Returned Goods Processor Responsibilities

Forwards any recalled/withdrawn product received in error, unopened and whole, to the Third Party Recall Service Provider for processing. Alternatively, may receive withdrawn product only and send it for destruction under GQAAC protocol when instructed — NALO QA ensures serial numbers are decommissioned before shipment for destruction (see GPR-501-5-115 / Tool 1).

## Distribution of Returned Goods

Goods returned from outside Lilly-controlled facilities as part of a Recall are never redistributed later, even if further investigation shows they're not in regulatory violation and are safe/effective. The global recall coordinator coordinates destruction of returned goods via an approved destruction method; destruction must be documented.

## NALO QA Responsibilities During Incorrectly Shipped Registry Governed Product Only

*(See Attachment B. These additional steps apply only when Registry Governed Product has been incorrectly shipped, due to associated regulatory requirements.)*

1. Identify the affected product/batch(es) (scope).
2. Identify their warehouse locations.
3. Ensure Operations cycle-counts those locations.
4. Determine distribution profile/dates by running ZS178 per batch (per USD-SOP-Processing GMP Data Requests-001, with SLED added to the view) — not necessary if the shipping destination is already known. The GBT report may also be used (content verified by QA/Materials/Operations before any agency submission — see the GBT site).
5. Ensure NASSC-CS runs its ZS178 variant including each customer's registration number, and that every customer has one recorded.
6. Initiate the NALO Task Force: Finished Good Returns; NASSC-CS; Enfield/PDC Operations (as required); NALO Quality; additional members as needed.
7. Initiate a Notification to Management per LQP101-1.
8. Discuss with GQAAC whether regulatory actions (3-day field alert, BPDR, etc.) are needed — NALO QA drafts any required.
9. Open an Observation/deviation.
10. Collect root-cause investigation data and identify preventive actions via Quality Risk Management principles.
11. Initiate an HHE per GQS132 if necessary.
12. Track receipt of product returned to the third-party return goods processor, ensuring serial numbers are decommissioned before shipment for destruction (see GPR-501-5-115 / Tool 1).
13. Complete the Observation/deviation within 30 calendar days — follow CQP-131-1 to complete its Form C.
14. Complete the action items in Attachment B.
15. Complete Attachment C and send to the Responsible Person, Eli Lilly Ireland Holdings Limited, every time a mock or actual recall occurs.
16. File all investigation documentation in the NALO GMP Library per the Global Records Retention Schedule.

## NALO Task Force Responsibilities for Incorrectly Shipped Registry Governed Product

1. Contact NALO Tech@Lilly early to make them aware — they may be able to help.
2. Ensure NASSC-CS has a "core" message for affected customers and replaces customer stock as quickly as possible.
3. Ensure the third-party returns goods processor knows expected product/batch return quantities for destruction, and that NALO QA has decommissioned serial numbers before shipment for destruction (see GPR-501-5-115 / Tool 1).
4. If serial numbers of incorrectly shipped product are requested, contact the NALO Operations Inventory Control Specialist.

---
*Source: USD-SOP-Recalls and Mkt Wdrawals-001, Version 19.0, Effective 28 Feb 2025. Approved 18 Feb 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
