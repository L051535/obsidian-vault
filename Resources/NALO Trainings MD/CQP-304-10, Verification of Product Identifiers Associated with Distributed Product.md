---
doc_number: PRD-18890
version: "1.0"
status: Effective
effective_date: 2021-07-01
training_id: CQP-304-10
tags:
  - nalo/sop
  - nalo/serialization
  - nalo/dscsa
---

# Verification of Product Identifiers Associated with Distributed Product

**Type:** Common Quality Practice (CQP)
**Approved by:** Ann Connery, VP Quality – Drug Product Operations Americas/Asia

## Purpose

Describes the process by which Lilly verifies serialization data at the request of external entities (wholesalers, pharmacies, hospitals, regulatory bodies, patients). "Verification" means verifying the **product identifier**: GTIN, serial number, batch number, and expiry date.

## Scope

Applied globally. Captures specific verification requirements from the **US Drug Supply Chain Security Act (DSCSA)**, including verification for suspect product investigation and saleable returns from authorized trading partners.

Pertains to distribution operations, and to finished products and drug/device combination products.

## 1.0 Verification Process Overview

Process steps: **Receive Request → Collect Information → Verify Requestor → Verify Serialization Data → Respond → Document the Request/Verification**

## 2.0 Receive Product Identifier Verification Requests

**2.1 Sources of Requests:** Wholesaler, Retailer–Pharmacy, Hospital, Healthcare Provider (HCP), Patient, Law enforcement, Regulator.

**2.2 Request from a Trading Partner in US:** Must meet the "Authorized Trading Partner (ATP)" requirement under DSCSA (valid license under State Law) to receive a response.
- Do not respond if the trading partner doesn't meet ATP criteria.
- Confirm authorized status per Section 4.0.

## 3.0 Collect Information from the Requester

**3.1 Requestor Contact Information** (business entities): name/phone/email of contact; company name, store number (if pharmacy), address, phone, corporate email; registration number (State License, Pharmacy License, DUNS, DEA, HIN, etc.); sales/purchase order if bought directly from Lilly.
- Patients/HCP/law enforcement/regulators: collect relevant contact info as provided.
- Store in **Global Customer Connect (GCC)**, except law enforcement/regulatory requests.

**3.2 Product Information:** Material Name, Dosage/Strength, GTIN, Serial number, Batch number, Expiration Date, and background of the request (question/concern, chain of custody, package quantity, photo if available). Request the identifier info electronically — a photograph is the ideal verification method. Store in GCC (except law enforcement/regulatory).

## 4.0 Verify the Legitimacy of the Requestor

Due diligence prevents fraudulent requests (e.g., phishing for copied/randomly generated serial numbers).

- **4.1 US Market:** Check State License validity via a third-party vendor database (e.g., MedProID), or search the Lilly PO number if purchased directly from Lilly. If not an ATP, notify **Global Security immediately**.
- **4.2 Other Countries:** Check authorized status where possible before responding. Notify Global Security if not an authorized trading partner.
- **4.3 Patients:** Understand the reason for the request; if purely verifying serialization, ask for proof of possession (e.g., a photo of the product).

## 5.0 Verify the Product Identifier

All four key data elements (GTIN, serial number, batch number, expiry date) must be verified together.

**5.1 Criteria for "Valid":**
1. The GTIN/serial/batch/expiry combination exists in Lilly's serial number management system.
2. System info matches the indicated product and dosage in the request.
3. Object Status of the serial number is not "Inactive."
4. The batch was released and destined for the country where the inquiry originated (helps identify diversion).
5. If a barcode photo is available, scan and cross-check against the human-readable text.

## 6.0 Respond to Requestor

Refer to **CQP-302-2 – Authentication Capabilities and Analysis of Suspect Products** for the correct response statement.

- **6.1 US Trading Partner:** Respond only if confirmed ATP (Section 4.0). Response due within 24 hours of receiving complete information; if incomplete, contact requestor within 24 hours for missing info. Requests outside Mon–Fri business hours must be addressed by the next business day. The 24-hour limit does not apply to patients/HCPs.
- **6.2 Other Countries:** Respond per country-specific requirements or within a reasonable time.
- **6.3 Valid serialization data:** Respond per CQP-302-2 plus a standard disclaimer — validity of the Product Identifier does not guarantee authenticity of packaging/product. Convey no other information.
- **6.4 Invalid serialization data:** Check for possible data entry/typing errors first. If confirmed invalid, treat as a **suspect product** and follow **LQP-112-5 – Requirements for Reporting Illegitimate Product and Falsified Medicines to Regulatory Authorities**. If tied to a product complaint, also follow **GQS130 – Product Complaints**.

## 7.0 Documenting Request and Verification

All requests/responses documented in **GCC**, retained per local regulatory record retention requirements — except law enforcement/regulator requests, which go in an appropriate document management system (e.g., SmartLab/CMS).

---
*Source: CQP-304-10, Version 1.0, Effective 01 Jul 2021. Approved 26 Jan 2021 by Ann Connery. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
