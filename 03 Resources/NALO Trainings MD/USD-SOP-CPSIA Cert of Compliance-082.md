---
doc_number: PRD-86812
version: "8.0"
status: Effective
effective_date: 2025-05-07
training_id: USD-SOP-CPSIA Cert of Compliance-082
tags:
  - nalo/sop
  - nalo/cpsia
---

# Maintaining and Providing Consumer Product Safety Improvement Act (CPSIA) Certificate of Compliance

**Areas Involved:** North American Shared Service Center – Customer Service (NASSC-CS), NALO Quality Assurance QualityDocs Coordinators

## Purpose

To provide guidance for maintaining Lilly CPSIA Certificates of Compliance and providing associated supplemental batch-specific information upon customer request.

## Reasons for Revision

Changed "NALO personnel" to "NASSC-CS personnel" in the introduction of the Processing Requests for CPSIA Certificate of Compliance section. Rationale: for clarification.

## References

- Global Records Retention Schedule
- USD-SOP-Domestic Launch Coord-016
- USD-SOP-Facility State Licensing-039
- USD-SOP-Create and Maintain Documents in Qdocs

## Introduction

Certificates of Compliance ("Certificates") are required by CPSA Sections 14a/14g (as amended by CPSIA), available on LillyTrade.com — covering general product info and child-resistant testing documentation required by the Poison Prevention Packaging Act (16 CFR §§ 1700.14(a), 1700.15) for all applicable Lilly drug products. Presented by product family and packaging configuration.

The US Manufacturer/importer or Private Labeler must certify the product under CPSA before it enters domestic commerce. If Lilly isn't the manufacturer/importer, Lilly must obtain a copy of that certificate plus a contact number for customer calls (see Maintaining Non-Lilly CPSIA Certificates of Compliance, below).

When Lilly (or a contract packager) packages a new product subject to CPSA/CPSIA, the MHS Trade and Specialty/Channel Mgmt Team requests a Certificate of Compliance from Lilly Packaging Engineering and Development, as part of the NALO Launch process (see USD-SOP-Domestic Launch Coord-016).

> [!note]
> Confirm the customer wants a CPSIA **Certificate of Compliance** (Consumer Product Safety Commission-regulated), not a manufacturing **Certificate of Conformance** (a quality-standards certification with product name, batch number, release date, expiration/retest date, tests performed with acceptance limits/results, and QA sign-off) — obtainable from Manufacturing via NASSC inventory coordinators. If unsure which the customer needs, contact NALO Quality Assurance.

The CPSC-Mandated Certificate is one page, optionally with a supplemental page:
- **Page 1** — the Certificate of Compliance for the given NDC, from LillyTrade.com.
- **Page 2** — provided only on request: supplemental batch documentation (packaging location and date for that batch).

Certification testing data is identical across all batches within a product family/packaging configuration, so batch-specific data is also identical across those batches.

> [!note]
> Attachment A (Supplemental Batch Specific Information) resides in the Electronic Document Management system (USD-SOP-CPSIA Cert of Compliance-082 Attachment A) for ease of use/revision.

## Processing Requests for CPSIA Certificate of Compliance

NASSC-CS personnel follow these steps. If the request is for a non-Lilly product Lilly distributes, give the customer the responsible company's contact info (found on the NASSC-CS collaboration space under "Contacts").

| Step | Action |
|---|---|
| 1 | Go to LillyTrade.com. |
| 2 | Using the customer-provided NDC Code, find and print the corresponding Certificate of Compliance. |
| 3 | Send a hard/scanned copy to the customer. If also requested, send the Supplemental Batch Specific Information (see next section). |

## Processing Requests for Supplemental Batch Specific Documentation

NALO personnel follow these steps when supplemental batch documentation is requested in addition to the basic Certificate.

| Step | Action |
|---|---|
| 1 | Ask the customer for: Product NDC and specific batch number (do not provide them a batch number); customer contact info (name, phone, title, company); and address/email to send information to. Record on Attachment A. |
| 2 | In SAP, determine packaging site/date for the batch: transaction BMBC → enter Batch → Execute → expand material to reveal Batch → double-click BATCH → Classifications tab → see "packaging site" → dropdown to enter the code found in "Vendor" → see result → Date tab for the date. |
| 3 | Ask the customer to repeat the NDC/batch number for verification. **If the batch can't be found in SAP or doesn't match the NDC:** mark "No" on Attachment A's "Batch/NDC ID Valid" line; complete the "Prepared by" and "Customer-Supplied Information" sections; forward the call to Lilly's call center (e.g., TLAC) to report the invalid batch/NDC as possible counterfeit product — **do not send the document to the customer**; notify NALO QA, who investigates per USD-SOP-Facility State Licensing-039; retain the document per Step 4 but don't send it. **If the batch number IS found in SAP for the NDC:** mark "Yes" on the Batch/NDC ID Valid line; record batch number, packaging site, and date; sign/date Attachment A; obtain second-person verification (a different person who can independently access SAP); contact NALO QA with any questions. |
| 4 | Send the customer a hard/scanned copy of the Certificate of Compliance AND the completed Attachment A. Keep a copy of the supplemental documentation filed in the CPSIA file cabinet in the NASSC-CS area's central filing cabinets, retained per the Global Records Retention Schedule. |

## Maintaining CPSIA Certificates of Compliance

When Packaging Engineering and Technology notifies that a new/revised CPSC-Mandated Certificate is available, the NALO QA QualityDocs Coordinator uploads it to QualityDocs:

| Step | Action |
|---|---|
| 1 | Check out the current document in QualityDocs. |
| 2 | Check in the new/revised document. Don't change the document footer unless the template itself changes (content-only changes don't need a footer change); check with MHS if unsure. |
| 3 | Schedule the reviewer(s)/approver(s) recommended by Packaging Engineering and Technology (typically: Consultant Engineer, Global Packaging Tech and Development; Sr. Director, Global Packaging; Manager, IDAP Packaging QA). |
| 4 | Once approved, send a PDF of the PowerPoint document (without the signature page) from the electronic document management system to the MHS Trade and Specialty/Channel Mgmt Team, whose IDS group uploads it to LillyTrade.com. |

## Maintaining Non-Lilly CPSIA Certificates of Compliance

On receiving a new/revised CPSC-Mandated Certificate and contact info for a non-Lilly product (from the Product Launch team or others), the NALO QA QualityDocs Coordinator:

| Step | Action |
|---|---|
| 1 | Scan the document and contact information; file the hardcopy in the GMP Library by CPSIA/product name. |
| 2 | Migrate a scanned copy into QualityDocs — either check out an existing document or route the new one for approval. |
| 3 | Place on a single approval workflow to migrate the approved document into QualityDocs; the NALO QA Coordinator is the sole approver for documents already approved on receipt. |

> [!note]
> Non-Lilly CoC documents are initial documentation only, not maintained as current — they represent Lilly's due diligence confirming the product owner's CPSIA compliance at the time received.

---
*Source: USD-SOP-CPSIA Cert of Compliance-082, Version 8.0, Effective 07 May 2025. Approved 14 Apr 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
