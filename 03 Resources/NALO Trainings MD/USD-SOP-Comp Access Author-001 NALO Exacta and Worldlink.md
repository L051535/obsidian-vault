---
doc_number: PRD-86845
version: "21.0"
status: Effective
effective_date: 2025-09-23
training_id: USD-SOP-Comp Access Author-001
tags:
  - nalo/sop
  - nalo/security
  - nalo/exacta
  - nalo/worldlink
---

# Computer Access Authorization for NALO Tech@Lilly Applications

**Supersedes:** 001-003113
**Areas Involved:** NALO Operations, NALO Tech@Lilly, NALO Quality Assurance
**Scope:** Worldlink, Exacta, AS/RS, and Autostore within NALO

## Purpose

Per Lilly Quality Standards and Computer System Validation (CSV) practices, outlines details supporting the Security Plan in the NALO validation package — granting, changing, or removing user access accounts.

## References

- LQP-302-2, Computer Systems
- LQP-302-29, Computer System and Platform Security
- NALO Security Plan

## Definitions

- **Security Administrator** – Assigned role within NALO Operations or NALO Tech@Lilly.

## Introduction

Access to secured validated computer systems must be controlled and documented via an application and training process.

**Attachments (in the electronic document management system):**
- Attachment A – NALO Application Security Form
- Attachment B – Autostore System Annual Security Access Review Form
- Attachment C – AS/RS System Annual Security Access Review Form

> [!warning]
> Attachment A/B/C are controlled parts of this procedure — not examples. Always print from the document management system at time of use to ensure the current version.

## Application for Accounts

NALO supervision steps to apply for a system account:

1. Ensure the applicant has a Lilly Systems User ID (e.g., `AB12345` Lilly, or `ABX1234` non-Lilly) — obtain via Computer Support Center, 317-277-7000.
2. Ensure proper curriculum assignment(s) based on intended system use (see Learning & Development; see NALO Security Plan for system role descriptions).
3. Process by system:

| System | Process |
|---|---|
| Exacta and/or Worldlink | Request access through **'MyAccess at Lilly'** (global account registration/access roster review system). |
| Autostore and/or AS/RS | Obtain Attachment A. Assist applicant with completion (confirm understanding of data/system confidentiality); verify applicant signature/date; sign/date for supervision approval; submit to the Security Administrator. |

## Establishing User Accounts (Security Administrator)

1. Upon receiving a request via ServiceNow/MyAccess or hardcopy, verify the requested action (add/change/delete).
2. **Exacta:** Log on to Exacta Manager → Administration/Operator Administration → enter operator type matching the "Access Requested" on Attachment A:
   - `EXOPR` – Exacta Operator
   - `EXPOW` – Exacta Power User
   - `EXOPR2` – Exacta Operator2 (weight check override)
   - `EXSYS` – NALO Tech@Lilly
   Submit → copy icon → enter user's system ID, password, name → Save.
3. **Worldlink:** Log on → Configuration > Users > Add → select user group (Enfield or Plainfield by description) → select Shipper/Power Shipper/US Dist IT → fill in user details (logon ID, name, location) → do **NOT** click "Prompt User to Change Password" → OK → Edit to open new user info.
4. **Autostore/AS/RS:** Log on → Enterprise Portal → Configuration & Maintenance > User Management > Users → New User → enter User ID (case sensitive), Full Name, Description (optional), Password/Confirm Password → OK → Edit: enter Email, Phone (optional), ensure "Enabled" checked, remove Voice Password check, select Security Role per form → Save. Sign/date Attachment A, retain in the Security Account Binder per the Global Records and Information Management Retention Schedule.
5. For MyAccess requests: document User Name, User ID, access type (new/change/removal), access level in the ServiceNow Task; complete and close it. For paper requests, proceed to notify.
6. Notify applicant and supervision by email that access is available (include new password if applicable).

## Temporary Access

Requested through the standard process. Removal is triggered by a CAPA tied to the end of the associated project.

> [!note]
> For access needed less than a day (e.g., third-party fixing a system outage), use a screen share with someone who already has access instead of provisioning a new account — end the screen share once resolved to end access.

## Application for Account Changes

| System | Process |
|---|---|
| Exacta or Worldlink | Confirm curricula if applicable; request modification via **MyAccess at Lilly** (`myaccess.lilly.com` while on the Lilly network). |
| Autostore or AS/RS | Obtain Attachment A; confirm curricula if applicable; mark the "Change" box and describe the change; sign/date for supervision approval; submit to the Security Administrator. |

## Changing User Accounts (Security Administrator)

1. Verify requested action from ServiceNow/MyAccess/hardcopy form.
2. **Exacta:** Operator Administration → enter "access requested" value in Operator ID (or select from list) → Submit → copy link → enter Operator ID/warehouse number → enter name → Save.
3. **Worldlink:** Configuration > Users > Users → find/select user → Edit → update user group if changing (Enfield/Plainfield; Shipper/Power Shipper/IT per Attachment A) → do **NOT** click "Prompt User to Change Password" → OK → Edit again to update Operator ID field per Attachment A.
4. **Autostore/AS/RS:** Enterprise Portal → Configuration & Maintenance > User Management > Users → find/select User ID → Edit → update fields → Save. Sign/date Attachment A, retain per retention schedule.
5. Document in ServiceNow Task (User Name, User ID, access type, access level) and close; or proceed to notification for paper requests.
6. Notify applicant and supervision by email that access has changed.

## Application for Inactivating Accounts

| System | Process |
|---|---|
| Autostore and/or AS/RS | Obtain Attachment A; complete Employee Name, Employee Logon ID, Global ID No, Department No; mark the "Inactivate" box; submit to the Security Administrator. |
| Worldlink and/or Exacta | Request inactivation through **MyAccess at Lilly**. |

## Inactivating User Accounts (Security Administrator)

1. Verify requested action from ServiceNow/MyAccess/hardcopy.
2. **Exacta:** Operator Administration → enter Employee Logon ID → select Warehouse Number → Submit → Edit icon → uncheck "Operator Enabled?" → Save.
3. **Worldlink:** Configuration > Users > Users → find user → change active flag from `Y` to `N`.
4. **Autostore/AS/RS:** Enterprise Portal → Configuration & Maintenance > User Management > Users → find/select User ID → Edit → uncheck "User Enabled" → Save.
5. Document in ServiceNow Task and close; or proceed to notification for paper requests.
6. Notify supervision (and applicant, if applicable) by email that access is inactivated.

## Annual Access Roster Review

Executed at least annually to confirm all active accounts are valid; process depends on whether MyAccess manages the review.

| System | Process |
|---|---|
| Exacta and Worldlink | Automated review notifications sent to supervisors via MyAccess on a scheduled basis. Revocation requests route to the Security Administrator, processed per the Changing/Inactivating sections. |
| Autostore | Complete Attachment B for all active accounts; submit to each supervisor for review/sign/date. Invalid accounts processed per Changing/Inactivating sections. Submit signed forms to NALO QA for review/sign/date. |
| AS/RS | Complete Attachment C for all active accounts; submit to each supervisor for review/sign/date. Invalid accounts processed per Changing/Inactivating sections. Submit signed forms to NALO QA for review/sign/date. |

## Reasons for Revision (current version)

- Removed Fresno. Rationale: CC40755631.

---
*Source: USD-SOP-Comp Access Author-001, Version 21.0, Effective 23 Sep 2025. Approved 03 Sep 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
