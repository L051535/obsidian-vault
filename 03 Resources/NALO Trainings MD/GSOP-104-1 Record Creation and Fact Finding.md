---
doc_number: PRD-128975
version: "1.0"
status: Effective
effective_date: 2025-07-14
training_id: GSOP-104-1
tags:
  - nalo/sop
  - nalo/deviation-management
---

# Record Creation and Fact Finding

**Associated Standard:** LQS104, Quality Deviation Management

## Purpose

To describe the responsibilities and activities related to executing Deviation Record Creation and Fact Finding, defining a consistent global approach across organizations aligned with LQS104.

## Scope

- **In Scope:** All areas whose job duties involve initiating deviations and completing initial fact finding, per LQS104 and associated procedures.
- **Out of Scope:** Departures that do not meet the definition of a deviation. Laboratory Investigations are out of scope of this procedure.

## 1.0 Procedure Overview

Refer to the Quality Management System (QMS) Vault Deviation Process Diagrams (SPEC-264498) for a detailed overview of this procedure.

## 2.0 Record Creation

> [!note]
> The source table's step-number column didn't extract cleanly from the PDF layout — activities below are listed in document order without forcing step numbers.

| Activity | Related Information | Responsible |
|---|---|---|
| Create Deviation Record in Global Deviation Database (GDD) | Create the record within one business day of the Date of Discovery; include a description. If the one-day timing is missed, justification/rationale must go in the record's Description field. Add the date of occurrence when known. When a deviation is initiated from a Laboratory Investigation or Quality Incident (e.g., Intracompany Issue), the system applies the Date of Discovery from the originating record. For Quality Deviations, see GSOP-104-6 (Laboratory Investigations) and [[GSOP-104-1-TL-5]] (Intracompany Issues). | Initiator |
| Deviation Due Date Assignment | Due dates are automatic: internal deviations get 30 calendar days from Date of Discovery; external deviations get 45 calendar days. For Quality Deviations, see GSOP-104-5 (External Deviations). | N/A (system-assigned) |
| Create Multi-Track Deviation Records | Condition: multiple Level 1 organizations involved. Select Multi-Track Deviation when the deviation impacts more than one Level 1 organization. See GSOP-104-1-TL-2 (Multi-Track Deviations). | Initiator, Fact Finder |
| Create Deviations with Unblinded Information | Condition: deviation touches systems/processes maintaining Clinical Trial blinding and details include subject treatment info — select "Yes" on "Does this Record Contain Unblinded Info?" If external party involvement applies too, use the Child Investigation record to manage that information. | Initiator, Fact Finder |
| Deviations Requiring External Collaboration | Condition: an external party needs to contribute to the record. Use the external collaboration feature in GDD. See GSOP-104-5-TL-1 (External Collaboration). | Initiator, Fact Finder |
| Deviation Initiated by External Party Organization | Condition: an external party initiated/caused the deviation and it must be tracked per agreements/contracts — use the External Deviation Process. See GSOP-104-5 (External Deviations). | Initiator, Fact Finder |
| Assign Team | The role for the next workflow action must be specified before advancing the record; other roles may be pre-populated. To complete Fact Finding, the Fact Finder and READY Assessor roles must be assigned — assigning the READY Assessor notifies the functional representative (e.g., Quality). | Initiator, Fact Finder |
| Assign Team for Unblinded Deviations | Condition: record contains unblinded information. Ensure everyone assigned (Fact Finder, Quality READY Assessor, Lead Investigator, Reviewers, Final/Quality Approvers) is permitted to view unblinded data by role or per the study's Blinding/Unblinding plan. Consult the project statistician or supervisor as needed. | Initiator, Fact Finder |

## 3.0 Conducting and Completing Deviation Fact Finding

**Responsible for this section:** Fact Finder

| Step | Activity | Related Information |
|---|---|---|
| 1 | Fact Finding Requirements | Add available data/facts so the READY Assessor has what's needed to correctly classify the deviation. |
| 2 | Batch Involvement | Select the appropriate Batch Involvement option and include batch bracketing rationale if applicable. For Quality Deviations, see [[GSOP-104-1-TL-1 Batch Bracketing]]. |
| 3 | Include Involved/Impacted Objects | Add objects involved in or impacted by the deviation to the appropriate sections. |
| 4 | Clinical Study Involved | Condition: clinical study involved — ensure the clinical study object is completed. |
| 5 | Impacted Country Object | Condition: IBUQA deviation — the Impacted Country object must list all impacted/involved countries where a Level 1 org is IBUQA. |
| 6 | Create User Tasks | Condition: additional info/task needed from other personnel — create and assign a User Task with details and a due date. |
| 7 | Conduct Interviews | Condition: an interview is needed — create and assign a User Task to the interviewer to document date, interviewee name/role, and a summary of what was discussed. |
| 8 | Create Impact Assessments | Condition: impact assessment(s) needed from other areas/functions — create and assign Impact Assessment records to impacted areas/functions (not needed if an org only impacts itself). |
| 9 | Create Quality Alerts | Condition: computer system event impacting non-IT business areas — contact appropriate personnel (e.g., BQA) to create/send a Quality Alert or Addendum. See GSOP-104-1-TL-3 (Quality Alerts). |
| 10 | Complete Fact Finding | Complete all relevant record fields; analyze the description and immediate steps for completeness. See GSOP-104-3-TL-5 (Artificial Intelligence for Deviations). Send the record to READY Assessment — see GSOP-104-2 (READY Assessment). |
| 11 | Address Rejection by READY Assessor | Condition: record rejected by READY Assessor for missing information — address the rejection and resend to READY Assessment. See GSOP-104-2 (READY Assessment). |

---
*Source: GSOP-104-1, Version 1.0, Effective 14 Jul 2025. Approved 13 May 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
