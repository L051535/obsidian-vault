---
doc_number: PRD-100114
version: "2.0"
status: Effective
effective_date: 2023-10-11
training_id: GLX-SOP-0012
tags:
  - nalo/sop
  - nalo/tms
  - nalo/bsr
---

# TMS - Production Support: BSR Process

**Associated GQS:** GQS304 (secondary: GQS301)
**Areas Involved:** Global Business Solutions
**Audience:** TMS End Users, Local Site QA/Global QA, LCCI TMS Master Data Team

## Purpose / Scope

Describes handling of SAP Business Support forms (BSR) — business service requests — and their components. In scope: **TMS Master Data Request**.

See [The SAP Business Support Site request types page].

## Start/Stop Points

Starts when a BSR is created; stops when the BSR is solved (closed).

## Definitions

- **SAP** – Integrated software modules spanning manufacturing, finance, sales, distribution, HR.
- **TMS** – Transport Management System.

## 2.0 Process Overview

### 2.1 BSR Created by Processor vs. Customer

BSRs should be created by the customer (or on behalf of someone else in the business). A processor may create one on the customer's behalf in some instances.

Always created by the customer:
- SAP Create Shopping Cart
- SAP or Non-SAP Security

### 2.2 BSR Forms — TMS Master Data Request

Required for TMS master data team support: lane setups, Locations (Airports/Seaports), one-time Location creation for Non-ERP orders, Schedules (Milkruns), Product pallet information, Container determination (Air Active/Passive & Ocean), Customer/Forwarder email IDs, email determination signature, Passive Temp Group, Temp Group, etc.

### 2.3 BSR Acceptance and Service Hours

| Area | Hours of Service | Hours of Service for Indy |
|---|---|---|
| TMS Master Data | 9am–5pm IST (Bengaluru) | 2am–10am |

- Regular working days only (Mon–Fri), no weekend coverage.
- Local holidays (India/US) may impact service hours.
- US summer shutdown and year-end shutdown impact US service hours.

### 2.4 Wrong BSR Chosen

Request type cannot be changed after initial selection — the BSR is rejected with a link/group pointer and rejection reason. For critical requests, the GBS processor submits the correct form on the customer's behalf.

**Contacts (Shared Inboxes):**
- TMS Master data team: DATA_TEAM_TMS@lilly.com
- TMS BPKC team: BPKC_TMS@lilly.com

### 2.5 Business Function / Subfunction Mismatch (SME Consulting BSR only)

Contact the should-be owner, confirm, then change the business/sub function. Inform the end user to choose the appropriate form next time.

### 2.6 Wrong Request Type Chosen

If editable at the SharePoint list level, the Processor changes the Request Type and informs the end user.

### 2.7 Priority

Determined by impact and urgency:

| Priority | Response Target |
|---|---|
| Critical | 2 hours |
| Urgent | 1 business day |
| Standard | 3 business days |
| Future | By "date needed," or as time permits |

### 2.8 BSR Notification and Monitoring

1. Automatic email notification on request creation; team distributes open requests by availability/customer/region.
2. Team reviews the SharePoint list regularly during the day.
3. Individual SharePoint alerts for Urgent/Critical requests (set up per person).

### 2.9 BSR Lifecycle

Statuses: **Open → Rejected / Cancelled by Requester / In Process → Awaiting Response → Awaiting Approval (2nd Person, GMP) → Awaiting Validation (1st Person Approver, GMP) → TMS MD Processing → Closed**

- **Open:** default status, not yet accepted.
- **Rejected:** not issued to the right team.
- **Cancelled by Requestor:** requestor withdraws the request.
- **In Process:** a processor has picked it up.
- **Awaiting Response:** waiting on requestor for clarifying details.
- **Awaiting Approval:** 2nd Person (Local QA) verifies processor's updates.
- **Awaiting Validation:** proposed solution needs user validation (not all teams use this); SC BSR exception — used to flag a secondary assignee for attention.
- **Closed:** completed. Do not reopen a closed BSR — ask the user to submit a new one (reopening affects SLA/reporting).

### 2.10 BSR Outputs

Main outputs relate to the nature of the BSR; some use root cause analysis, new issue identification, or repetitive issue identification.

### 2.11 Systems Supported

| BSR | System |
|---|---|
| Transport Management System (TMS) | SAP TMS (module within SAP) |

### 2.12 BSR Closing

1. Add comments/solution to the processor's field.
2. Attach any email chains outside BSR comments.
3. If the user doesn't respond after two reminders on solution acceptance, close the BSR.
4. Choose "Close" status and save.

### 2.13 Job Aids

- SME Consulting BSR: training available on the BSR site.
- TMS: additional Job Aids for requestor/process on SharePoint.

### 2.14 References

- TMS: BSR Requestor Work Instructions
- TMS: BSR Processor Work Instructions
- TMS: BSR Local QA Approval list (Local QA Approver List_Version_01.xlsx)

### 2.15 Request Types — Tower: Transport Management System (TMS)

**TMS Master Data Request** covers: Lanes/Schedules/Milkruns, Product–NPI/Air Active/Air Passive/Air Loose container determination, Ocean Container Determination, Shipping Documents, Temp Group, Passive Temp Group, Customer Email ID Determination, Forwarders Email ID determination, Location (Airport/Seaports/Hubs), TMS SME Consulting.

| Request Type | Details | Mandatory Fields |
|---|---|---|
| Lanes (GMP Critical-Local) | New Lanes, Existing Lane changes (carrier), New Milkruns, Milkrun changes | Shipping point, Destination country, Primary Carrier, Mode of transportation, Milkrun schedules, Duration & Distance (Road lanes), Means of transport (LTL/FTL/Van) |
| Product (GMP Critical-Local) | New Product Launch, changes to Pallet type/Normalization Qty/Handling units/Container determination (Air Active/Passive/Loose) | Shipping point, Destination country, Product/SKU no, pallet type (EU/US), SKUs/Pallet, Handling Unit, Container Det, Temp Group, Passive Temp Group, Min/Max Qty, Capacity, Material Number |
| Ocean Container Determination (GMP Local) | Conditions for determining ocean container | Source Location, Destination Country, Temp Group, Min/Max Qty, Container type |
| Shipping Documents (GMP Local) | Maintenance of shipping docs (Bill of Lading, AWB, etc.) | Location, country key, mode of transport, Req Doc type, document description |
| Temp Group (GMP Global) | New/changed temp group for product — requires Global Quality approval | — |
| Passive Temp Group (GMP Critical-Local) | List of passive temp groups maintained at site | Refer to attachment template |
| TMS SME Consulting | Training/documentation, IT configuration support, BPKC support | Detailed description of the request |
| Customers Email ID (Non GMP Critical) | List of customer email IDs | Refer to attachment template |
| Forwarders Email ID (Non GMP Critical) | List of forwarder email IDs | Refer to attachment template |

## Revision History

| Version | Effective Date | Major Changes |
|---|---|---|
| 01 | 15-Nov-2021 | New document |
| 02 | See DMS | Changes to request types & new templates; redefined GMP request handling and approval workflow |

---
*Source: GLX-SOP-0012, Version 2.0, Effective 11 Oct 2023. Approved 27 Sep 2023. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
