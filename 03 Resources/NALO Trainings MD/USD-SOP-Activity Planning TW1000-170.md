---
doc_number: PRD-86842
version: "3.0"
status: Effective
effective_date: 2025-11-01
training_id: USD-SOP-Activity Planning TW1000-170
tags:
  - nalo/sop
---

# Use of the TW1000 Activity Planning Module in NALO

**Areas Involved:** North American Logistics Operations (NALO), NALO Quality Assurance, NALO Health, Safety and Environmental, and NALO support areas

## Purpose

To describe the process for using the Activity Planning module in TrackWise1000.

## References

- Global Records Retention Schedule
- Global CAPA site for TrackWise guidance and resources: Trackwise 1000 - Home (lilly.com)

## Reasons for Revision

No revisions were necessary at periodic review.

## Definitions

| Term | Definition |
|---|---|
| Activity Planning Record | A Trackwise electronic record such as Work Plan, Work Item, Work Task, or Work Action housed in the Activity Planning module of TrackWise. |
| Amendment Request | Review Owner initiated request to modify an approved record. |
| Effectiveness Check | A field in a Work Action child record used to document the outcome and/or impact of the action plan — an assessment of how well it addressed the inspection finding(s). |
| Work Plan Record | A TrackWise electronic parent record identified by a unique number, utilized to group related Work Items together (e.g., 2022 Standard Gap Assessments). |
| Work Item Record | A TrackWise electronic parent or child record identified by a unique number, utilized to capture the completion of an activity (e.g., assess the GQS203 revisions to determine impact to NALO and any actions required to close gaps). |
| Work Action Record | A TrackWise record identified by a unique number, utilized to track the implementation of actions related to or resulting from a work item (e.g., revise procedure ABC to close a gap identified during a gap assessment). |

## Introduction

The Activity Planning module within TrackWise is utilized to ensure that Action Items from various sources are completed by the agreed upon due date. See Attachment A, TW1000 Activity Planning Module Workflow for detailed workflow.

Work Plans, Work Items, Work Actions, and Work Tasks within scope can be developed from GMP approved reviews, including but not limited to: Annual Product Reviews, Local procedures, Management Reviews, Quality Plan, Risk Assessments, Periodic Supplier Performance Reviews, Pest and Temperature Monitoring, Self-Inspections, Observations from Inspection/Audits, Issues from self-inspections, Inspections, Reviews of facilities/utilities/equipment/computer systems, Gap Assessment Actions, Recurring Actions Required by Procedure, Actions due to regulatory or customer commitments, and Outstanding Items from Qualification or Validation Reports.

Effectiveness Checks are only required for Work Items and Work Actions resulting from inspections/audits; optional for other records.

Cancellation/Abandonment of Activity Planning records can be performed by quality on records that are in an open state or in progress depending on the workflow. Justification for abandonment is provided within the Execution Summary field.

All Quality-related Work Actions must be verified by a Quality Verifier. HSE Work Actions must be verified by an HSE Verifier. All Effectiveness Checks must be executed by a Quality or HSE Verifier for inspection/audit CAPA — the Record Owner must be notified on any Effectiveness Check deemed ineffective.

## Responsibilities

- **Work Item, Work Action or Work Task Owner** – Executes and documents completion of assigned records. Requests Due Date Changes.
- **Activity Planning Approver** – Reviews and approves Activity Planning records and due date changes. Ensures records fall within scope of this procedure and that actions from approved GMP reviews are entered accurately. Quality approval is required for all quality records; HSE approves HSE records.
- **Activity Planning Owner** – Creates Work Plan, Work Action, Work Tasks, or Work Items. Creates Effectiveness Checks for inspection-related CAPA. Submits records for approval, originates due date changes, abandons records with justification where applicable, reviews completed Work Task records, and communicates ineffective Effectiveness Check outcomes to the Quality or HSE Lead Team.
- **Verifier/Evaluator** (QA or HSE representative or higher) – Verifies completed Work Action records, conducts evaluation of Effectiveness Checks for inspection-related CAPA, notifies Record Owner upon ineffectiveness and assists with resolution.

## Activity Planning Records

| Step | Action |
|---|---|
| 1 | The Activity Owner determines the type of Record to be created based on the specific information that needs to be completed. |
| 2 | Activity Planning records can have CAPAs associated with them. |
| 3 | When applicable, the Activity Owner indicates in the Work Item record that an Effectiveness Check record is required; assigned to a Verifier. |
| 4 | Activity Planning records can have both Area and Quality, or HSE, approvals depending on record type. |
| 5 | Record owners ensure documentation is complete within the record. |
| 6 | Activity Records can be used to create a schedule of periodic required related activities. |

> [!note]
> If Quality is the only functional area approving the Work Item, their name should be selected under Organizational Unit Approver, as Organizational Unit Approver is a required field for TW1000. If the Work Plan is an annual Plan, the last Work Item in the plan should be to create and approve next year's Work Plan.

## Creation of Work Plan Records

Work Plan records are parent records and must have Work Items associated with them. There are four categories of Work Plans:

| Work Plan Category | Description | Pre-Execution Approvers |
|---|---|---|
| Calendar | Used to create items due by certain dates (Quality Plan, Annual Product Review Schedule, Management Review) | Per corresponding procedure's minimum approvers; if none, Quality and Org unit approval at minimum. |
| Impact Assessment | Used to create items to assess changes in global standards or regulations | Per corresponding procedure's minimum approvers; if none, Quality unit approval at minimum. |
| Periodic Review | Used to create items documenting completion of periodic reviews (e.g., SOPs, job descriptions) | Per corresponding procedure's minimum approvers; if none, Quality unit approval at minimum. |
| Site Self-Assessment/Inspection | Used to create a site self-inspection schedule or track actions from internal/external inspections | Site Quality Leader |

Work Plan records are not assigned an owner. At least one Work Item must be associated with a Work Plan upon creation, created via the Work Item Grid. Work Plans are approved and closed upon approval in TrackWise. Additional Work Items can be added after the Work Plan is closed.

## Work Item Records

Work Item records are parent records and can be created independently or as part of a Work Plan record. NALO utilizes the following categories (partial list — see full table for pre/post-execution approvers):

- Access Roster Review, Annual Product Review, Asset Qualification Monitoring, Calendar Item, Compliance Action, Continuous Improvement, Impact Assessment, Periodic Review, Process Monitoring Recurring Calendar Item, Recurring Calendar Item.
- **Not used by NALO:** Data Integrity Assessment, Deviation Quarterly Review, Process Hazard Review, Site Self-Assessment/Inspection (as a Work Item category).

Notes on Work Item records:
- Assigned an owner in TrackWise.
- Approved prior to execution and after execution.
- Can be abandoned after creation with appropriate justification, approved by original approvers of the Work Plan/Item and a Quality Assurance representative or higher.
- Due Date changes require justification and approval by a Quality Assurance representative or higher; changes for Work Items associated with APRs, Quality Plan, Inspections, Audits, or Management Reviews must be approved by the associated Work Plan approvers.
- Additional records such as Work Task and Work Actions can be created as part of this record.
- Effectiveness Checks can be assigned as part of this record where applicable — Site Self-Assessment/Inspection work items require an Effectiveness Check.
- The Work Item Title field is limited to 120 characters and is not editable after approval (input is not blocked at the limit, but the title truncates post-approval — a clear title is critical). The Work Item Description field is editable after approval and has no character limit.

## Work Task Records

| Step | Action |
|---|---|
| 1 | The Work Task is an optional record used to contact subject matter experts to complete a portion of the work documented in the Work Item. |
| 2 | Created using the work task grid in TrackWise; multiple records can be created at once or individually. |
| 3 | Work Task records are assigned an owner in TrackWise. |
| 4 | Due dates can be pushed out as long as they remain within the approved due date of the parent Work Item record; exceeding dates require justification and approval. |
| 5 | Work Task records can be cancelled after creation with no approval. |
| 6 | Work Task records are approved and closed with the post-approval of the Work Item. |

## Work Action Records

| Step | Action |
|---|---|
| 1 | The Work Action record is a child record of the Work Item record, used to close identified gaps or actions for continuous improvement. Executed after the post-approval of the Work Item. |
| 2 | Work Action records are assigned an owner in TrackWise. |
| 3 | Can be abandoned after post-approval of the Work Item with appropriate justification and approval by a Quality Assurance representative or higher. |
| 4 | Due Date changes require appropriate justification and approval by a Quality Assurance representative or higher. |

## Verification of Work Actions

| Step | Action |
|---|---|
| 1 | Action verification and record closure shall be completed using Attachment B, TW1000 Activity Planning Tool. An individual may not verify a Work Action they have completed/performed. |
| 2 | Verification of all actions must be completed by a Verifier. |

## Effectiveness Check Evaluations

| Step | Action |
|---|---|
| 1 | Effectiveness Checks must be completed by a Verifier. |
| 2 | The Effectiveness Check Target Due Date is set once all related actions are completed. Sufficient time should be included to acquire enough data to support the Action Plan's success. Contact the Activity Owner if needed. |

> [!note]
> An Effectiveness Check should be used for actions related to inspections and/or audit findings (internal or external) when applicable.

---
*Source: USD-SOP-Activity Planning TW1000-170, Version 3.0, Effective 01 Nov 2025. Approved 25 Sep 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
