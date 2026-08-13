---
doc_number: PRD-130655
version: "2.0"
status: Effective
effective_date: 2026-08-10
training_id: USD-SOP-NALO US DSCSA Exceptions Management
tags:
  - nalo/sop
  - nalo/dscsa
---

# US Drug Supply Chain Security Act (DSCSA) Exceptions Management in North American Logistics Operations (NALO)

## Section 1 — Document Administration

### 1.1 Purpose and Scope

For compliance and patient safety, describes how NALO resolves DSCSA exceptions — including discrepancies between physical products and electronic transaction data.

### 1.2 Areas Involved

North American Shared Service Center – Customer Service; NALO Tech@Lilly; NALO Operations; Serialization SAP Tech@Lilly; Global Serialization & Packaging (GSP); Lilly Value & Access; NALO Quality Assurance; Global Quality Manufacturing Logistics; Consumer Product Quality Assurance (CPQA).

### 1.3 Approvals and Signatories

Management from each of: North American Shared Service Center – Customer Service; NALO Tech@Lilly; NALO Operations; Serialization SAP Tech@Lilly; Global Serialization & Packaging (GSP); Lilly Value & Access; NALO Quality Assurance; Global Quality Manufacturing Logistics; Consumer Product Quality Assurance (CPQA).

### 1.4 Revision History

- **Version 1.0** (Oct 2025) — new procedure.
- **Version 2.0** — added ServiceNow (SNOW) reporting requirements (incl. the DSCSA AI SNOW Automation Tool), the overall triage-flow context, the process for exceptions with no data available, the process for managing swaps (incl. inventory verification for suspected swaps), deviation determination criteria, and documentation/closure requirements.

### 1.5 Related Documents

- SRL-SOP-GSP Data Broker Error Management
- GQS105, Data Integrity and Good Documentation Practices
- USD-SOP-NASSC_CS Sales Order for Trade Product Entry-012
- GPR-501-5-109, Serialization TraceLink Partner Setup
- GPR-501-5-115, Decommissioning Serialization Material
- LQP-112-5, Requirements for Reporting Illegitimate Product and Falsified Medicines to Regulatory Authorities
- Product Quality Complaint (PQC) Complaint-Handling SOP

## Section 2 — Definitions, Acronyms, and Background

### 2.1 Regulatory Background — DSCSA and EDDS Requirements

The US FDA Drug Supply Chain Security Act (DSCSA) improves/ensures pharmaceutical supply chain safety and interoperability. Its Enhanced Drug Distribution Security (EDDS) requirements require unit-level tracing through the supply chain via serialization — once EDDS is in effect, Transaction Information (TI) must contain serial numbers matching the physical product shipped. NALO manages exceptions (shipping errors, data issues) promptly to: successfully investigate/resolve issues; avoid wholesaler quarantine and distribution pauses; avoid market shortages; and avoid wholesalers returning product over incorrect/missing data.

### 2.2 Definitions and Acronyms

> [!note]
> This table's term/definition columns were offset in PDF extraction; the pairings below were reconstructed by matching each definition to its term in sequence order (verified against context) — verify against the source PDF if precision matters.

| Term | Definition |
|---|---|
| AS2 | Applicability Statement 2 — Business-to-Business secure messaging protocol for transmitting EDI documents between organizations. |
| ASN | Advanced Shipping Notice — submission of batch-level details to an order's recipient from the shipping location. |
| ATTP | Advanced Track & Trace for Pharmaceuticals — source of all serial numbers/ranges; distributes numbers to packaging lines, palletizers, and the warehouse. |
| CARTS | Complaints and Remarks to Suppliers (and Service Providers) — a formal quality communication tool notifying suppliers of a confirmed or potential issue. |
| DSCSA | Drug Supply Chain Security Act. |
| EPCIS | Electronic Product Code Information Services — industry standard for exchanging serialized data. |
| GLN | Global Location Number — standardized identifier for a company or specific company location. |
| GS1 | International non-profit standards organization. |
| GSP | Global Serialization Program. |
| GTIN | Global Trade Identification Number. |
| ICI | Intracompany Issue — a pre-deviation step enabling structured communication between sites, helping assess whether a deviation is needed. |
| LedgerDomain | Third-party software owner used for receiving serial number verification requests. |
| Lilly Sales Order | SAP sales order including sold-to, ship-to, products, and quantities. |
| LVA | Lilly Value & Access. |
| NALO | North American Logistics Operations. |
| NTM | Notice to Management — process for escalating issues. |
| Product Complaint | Any communication alleging a deficiency in a Lilly product's identity, quality, durability, reliability, safety, effectiveness, labeling, packaging, or performance after release for distribution/use. |
| QMS | Quality Management System — integrated global system of processes, standards, tools, and governance. |
| REQ | Request — the overall request submitted via a ServiceNow request. |
| RITM | Requested Item — the specific thing being asked for within a ServiceNow request. |
| SAP | Global Enterprise Resource Planning system. |
| Serial Number | Unique number for each GTIN identifying a saleable unit. |
| SNOW | ServiceNow — Lilly's enterprise platform for submitting/tracking/managing/resolving work requests and issues. |
| SGTIN | Combination of GTIN and Serial Number, used to confirm a valid serial number in SAP. Syntax: `01[GTIN]21[Serial Number]`. |
| TLAC | The Lilly Answers Center. |
| TraceLink | Third-party cloud Data Broker platform managing all external partner data exchange for serialized product distribution. |
| WMS | Warehouse Management System. |

### 2.3 Key Systems Overview — SAP and TraceLink Purpose

Lilly uses TraceLink (Life Sciences Cloud) as its DSCSA solution, generating T3s (combined Transaction Information, Transaction History, and Transaction Statement documents) and their electronic storage per FD&C Act §582(b)(1)(C). It conforms to lot-based tracking industry standards and uses the HDA extended EDI 856 ASN for exchanging compliance information.

Using Lilly-administrator-entered data and Lilly Master Data defined during onboarding, TraceLink generates complete, compliant transactions. Lilly's SAP master data configuration systematically triggers the interface to TraceLink for data elements supporting a product's first commercial shipment; TraceLink then maps that data to EDI ASN and EPCIS formats for wholesale distribution, per DSCSA transaction-tracing requirements.

## Section 3 — Roles and Responsibilities

The **DSCSA Triage Team** is the first-line, cross-functional group receiving, assessing, coordinating, and driving resolution of DSCSA exceptions/verification requests to restore product-data alignment and maintain compliance/audit readiness.

| Role / Team | Key Responsibilities |
|---|---|
| Exceptions Resolution Coordinator – Intake | End-to-end orchestration of a triaged exception; progress tracking across systems; drives closure communication with wholesalers; ensures documentation completeness. |
| SAP ATTP Tech@Lilly | Investigates/corrects serialization and aggregation issues in ATTP — fixing missing/mismatched serial data, retriggering EPCIS without reversing PGI to restore data alignment. |
| Data Broker Tech@Lilly | Investigates/resolves TraceLink-related issues — validating EPCIS/ASN transmissions, troubleshooting AS2/MDN messaging failures, confirming serialized data delivery and partner connectivity. |
| NALO Customer Service | Enables commercial/financial resolution of DSCSA exceptions — manages credits, returns, reverse-logistics outcomes when data correction alone can't resolve compliance issues. |
| NALO Operations and 3PL Operations | Validates physical warehouse activities (pick, pack, ship, swaps, relabeling, inventory movements); confirms whether exceptions are operational vs. data/system-driven; helps DSCSA triage perform inventory counts; logs a deviation whenever a procedure isn't followed and leads to a DSCSA exception. |
| NALO Quality Assurance | Provides compliance oversight to DSCSA triage; ensures proper deviation determination, documentation integrity, audit readiness; supports regulatory responses and quality governance. |
| LVA Channel Operations and Trade Accounts | Manages trade/customer-facing implications of exceptions; ensures resolutions are communicated/executed in ways aligned with wholesaler relationships and commercial policy. |
| Global Serialization Program (GSP) | Provides serialization SME support; helps interpret serialization/aggregation behavior; assesses commissioning/decommissioning/rework scenarios; evaluates whether an exception is serialization-logic vs. operational-execution driven. |
| Global Quality – DSCSA | Accountable for overall DSCSA compliance and complete accuracy between physical and system inventory/serial number representation; accountable for the quality-control recording process that minimizes future exceptions. |
| Consumer Product Quality Assurance (CPQA) | For product returns due to a DSCSA exception, facilitates the return process from the downstream customer. |

## Section 4 — Exception Intake and Communication Standards

### 4.1 Single Intake Channel — Required Email Address and Routing Rules

All trade-partner exceptions go to **Lilly_Data_Broker_DSCSA@lists.lilly.com** — this single funnel is critical for issue awareness and resolution accountability/speed/priority. Requests received internally through other pathways must be immediately redirected to this inbox; work initiated outside this channel should be redirected back to the Triage Team.

### 4.2 Exception Categories Defined

- **Data Transmission Errors** — missing/incorrect transaction data (e.g., EPCIS files, ASN mismatches); the most common type, usually resolved by resending corrected data.
- **Product/No Data** — physical product received but serialized data missing/delayed; can trigger product holds until reconciled.
- **Data/No Product** — serialized data received but physical product isn't; may indicate a shipping or inventory error.
- **Product/Data Mismatch** — serialized data doesn't match the physical product (wrong GTIN, serial number, or lot); can lead to quarantine or returns.
- **Packaging/Labeling Issues** — damaged/unreadable barcodes, incorrect labeling, or missing identifiers preventing verification.

## Section 5 — Overall Triage Flow and Process

### 5.1 Overall Triage Flow and Initial Evaluation

The master process map for every DSCSA exception's lifecycle — from receipt through corrective/preventive action to closure — governing how this SOP's sub-procedures sequence and interconnect. All exceptions follow this single flow (see Attachment A).

| Step | Phase | Activity |
|---|---|---|
| 1 | Intake | Receive the incoming exception email from the customer via Lilly_Data_Broker_DSCSA@lists.lilly.com. |
| 2 | Intake | The DSCSA AI ServiceNow Automation Tool automatically processes and creates the appropriate records (INC, INCTSK) in ServiceNow for every new exception. |
| 3 | Triage | Decision: are we able to provide the data? |
| 4 | Returns (NO) | Log a complaint with TLAC to initiate the return process with CPQA. |
| 5 | Corrective Actions (YES) | Start the investigation process to find the root cause. |
| 6 | Corrective Actions | Once root cause is identified, confirm the steps to resolve the DSCSA data exception. |
| 7 | Corrective Actions | Take the necessary steps in SAP ATTP/S4/TraceLink to revise the data and send EPCIS data to the customer. |
| 8 | Corrective Actions | Retrieve confirmation from the customer of receipt/acceptance of the data. |
| 9 | Preventive Actions | Decision: was the root cause a process issue or a system issue? |
| 10a | Preventive (System) | Enter the needed Incident/RITM in SNOW, if applicable. |
| 10b | Preventive (Process) | Decision: was this caused by an internal or external site? |
| 11a | Preventive (Internal) | Create a deviation/ICI covering the exception (or add to an existing one). |
| 11b | Preventive (External) | Create a CARTS for the supplier (or add to an existing one). |
| 12 | Closure | Close the incident with the correct resolution code, likely cause, and deviation/ICI/CARTS reference number (if applicable). |

### 5.2 Use of ServiceNow to Manage the Process

Every email received at the intake address must be formally tracked — the Lilly team creates a ServiceNow Incident for each exception, accurately reflecting the details submitted by downstream partners, to ensure a timely, appropriate response.

## Section 6 — Exception Triage Workflow

### 6.1 Exception Types

**Type 1: EPCIS/ASN Data Missing for Entire Shipment (Truck, No Data)**
- Check TraceLink to confirm delivery; if not delivered, investigate further.
- ZSHP is sometimes retriggered to resend the data.
- TraceLink sends the ASN — if it isn't sent, follow up and investigate on the TraceLink/SAP side.

**Type 2: Product Received, No Serialized Data (Product/No Data)**
- May result from incorrect packaging labels, physical swaps between deliveries, or overage — swaps can occur at the carton/case or pallet level.
- For overage: NALO Trade Claims receives the relevant claims; the DSCSA team provides data matching the overage claim; NALO Trade Claims processes a rush order without a pick delivery.
- For shipments missing an SGTIN label at the pallet/case level: ask the customer to scan at a lower packaging level.

**Inability to Provide Serialization Data** — when data can't be provided, a return is processed instead, split into two buckets:
- **Product Complaint** (see Attachment B) — DSCSA Triage collects data and assesses return eligibility (e.g., unscannable product, decommissioned product received). DSCSA initiates a Product Complaint (PC) with TLAC within 24 hours to start a wholesaler return. TLAC submits the PC for CPQA to process; CPQA updates the customer record and coordinates the return; NALO Trade Claims administers customer credit once the return is verified; the DSCSA Team decommissions the serial number if applicable.
- **Saleable Returns Linked to a Different Wholesaler** (see Attachment C) — DSCSA Triage verifies shipment to determine if the product went to an alternate customer, obtaining delivery info from the requesting customer. If the customer can't provide valid delivery info, inform them Lilly cannot supply DSCSA transaction data for that product.

**Type 3: Data Received, No Physical Product (Data/No Product) — Shortage**
- NALO Trade Claims provides the list of serial numbers identified as short.
- NALO Trade Claims collaborates with NALO Operations and 3PL to confirm the shortage.
- Once confirmed, the Data Broker Team initiates an incident to reactivate those serial numbers in both TraceLink and SAP ATTP.

**Type 4: Master Data Exceptions**
- When EPCIS isn't delivered due to GLN or NDC format issues, the SAP/TraceLink teams resolve master data issues via the Incident process.

## Section 7 — Quality Oversight and Deviation Determination

A **deviation** is a departure from approved processes, procedures, instructions, specifications, or regulatory requirements — including an unexpected occurrence with no procedural response (no existing SOP step, work instruction, or decision branch applies).

Each functional area investigates and determines whether a deviation is needed, creating the record if so; where functional ownership is unclear, the Exceptions Resolution Coordinator executes the deviation record. **Not every exception is a deviation** — routine exceptions resolved through standard triage (e.g., a successful data re-send, a trading-partner configuration fix, or a confirmed non-Lilly issue) don't require one.

> [!warning]
> The source PDF includes a matrix mapping each exception type to its deviation-trigger condition, root-cause category, and resulting quality record type. The table's rows and columns were misaligned in PDF extraction (more condition rows than exception-type labels, with no reliable way to re-pair them from context), so it isn't reproduced here to avoid guessing at compliance-relevant mappings. Exception types referenced in the matrix: Product/No Data, Product/Data Mismatch, Data Transmission, Label/Packaging, Data/No Product, Swap/Incorrect Shipment, and No Procedural Response. **Refer to the source PDF/QualityDocs directly for the exact per-type deviation triggers, root-cause categories, and quality record types.**

## Section 8 — Escalation Pathway

Defines the tiered escalation framework from initial resolution through FDA reporting — the criteria for advancing a tier, required activities/documentation at each level, and standard exception-type definitions. Escalation is progressive: not every exception advances beyond Tier 0. Five tiers (0–4) represent increasing organizational response, quality system engagement, and regulatory exposure; exceptions enter at the tier matching their severity and may advance as investigation findings warrant.

| Tier | Escalation Level | Trigger Criteria | Key Activities | Documentation |
|---|---|---|---|---|
| 0 | No Response Needed | Timely issue resolution within DSCSA timing requirements — no further response required. | Standard triage and resolution per exception-type procedures. | — |
| 1 | ICI/CART/RITM or SNOW Response | Exception requires formal tracking but can be resolved without quality system escalation. | Create/manage an ICI, CART, RITM, or SNOW record; coordinate with the responsible party for resolution. | Issue tracking in TraceLink required; ICI/CART/QMS/SNOW/RITM numbers tracked in the SNOW record. |
| 2 | Deviation Initiation | Exception meets the deviation threshold (departure from approved process, GxP, or regulatory requirement). | Initiate a deviation per the NALO Quality Assess/Review/Investigate/Approve workflow. | Deviation record in the quality system, with reference number included in the SNOW record. |
| 3 | Escalation Response – Stakeholder Meeting | Deviation investigation reveals potential impact to product quality, patient safety, or supply chain integrity requiring cross-functional decision-making. | Convene an escalation stakeholder meeting; assess scope of impact; determine if FDA engagement is required. | — |
| 4 | 3911 FDA Reporting/Consult | Confirmed impact to product integrity, or confirmed illegitimate product, requiring FDA notification. | Initiate the 3911 FDA reporting process and/or FDA consult; execute the NTM process if applicable. | Documentation in QDocs/FDA website. |

## Section 9 — Incident Closure and Post-Resolution Requirements

Once resolved:
- Close the incident only after the exception is fully resolved.
- Confirm and record customer satisfaction with the resolution.
- Initiate a deviation or CARTS record if applicable, with its reference number in the SNOW record.
- If DSCSA data can't be furnished, begin the return process and include the Product Complaint number in the SNOW record.

## Section 10 — Serial Number Decommissioning

- The DSCSA Triage Team initiates a Request Item (RITM) to the NALO Quality Team to decommission the applicable serial number(s) in SAP, assigned to group **APPS-SUPP-US-NALO-DSCSA-EXCEPTIONS**. RITM details must correctly reference the original ICI/Deviation/PC number to link the decommissioned serials to the exception. *Serial numbers must be in SGTIN form to execute SAP decommissioning.*
- An RITM Task is created concurrently, assigned to the TraceLink Data Broker Team (group **DATABROKER-SERIALIZATION-GLB**), which confirms serial number status in TraceLink only after successful SAP decommissioning.
- These SAP steps follow GPR-501-5-115 (Global Decommissioning Serialization Material SOP).

## Attachments

> [!note]
> Attachments A–C are process diagrams that didn't extract as text — refer to the source PDF/QualityDocs for the visuals.

- **Attachment A** — DSCSA Exception Management Process High Level Overview
- **Attachment B** — Product Complaint Product Return
- **Attachment C** — Saleable Return Product Return

---
*Source: USD-SOP-NALO US DSCSA Exceptions Management, Version 2.0, Effective 10 Aug 2026. Approved 29 Jul 2026. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
