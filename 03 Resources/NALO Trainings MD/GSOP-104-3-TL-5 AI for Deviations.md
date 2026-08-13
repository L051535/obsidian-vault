---
doc_number: TL-340525
version: "1.0"
status: Effective
effective_date: 2025-07-14
training_id: GSOP-104-3-TL-5
tags:
  - nalo/sop
  - nalo/deviation-management
---

# Artificial Intelligence for Deviations

**Associated Procedure:** GSOP-104-3, Investigation

## Purpose

Describes the process of using Artificial Intelligence (AI) for deviations — tools that review/give feedback on deviation description and immediate actions, categorize root cause(s), review/give feedback on investigation details, and generate executive summaries for deviation reports.

## Scope

- **In Scope:** Quality Deviations and Lab Investigations. The Virtual Mentor Tools, Root Cause Categorization Tool, and Executive Summary Generator are available for Quality deviation records; only the Root Cause Categorization Tool is available for lab investigations.
- **Out of Scope:** External deviation records needing no further investigation (out of scope for Root Cause Categorization, Virtual Mentor for Investigation, and Executive Summary Generator). Non-deviation quality record types. Private records are not accessible to the AI tools at all.

## 1.0 Overview

AI tools aid deviation investigation and documentation. **Use of the Root Cause Categorization Tool is required**; the other tools are recommended but not required. For every tool's output, the user must review results for accuracy and make any necessary corrections before finalizing the deviation record.

## 2.0 Virtual Mentor for Fact Finding

**Responsible for this section:** Fact Finder

| Step | Activity | Related Information |
|---|---|---|
| 1 | Use Virtual Mentor for Fact Finding | Analyzes the description and immediate steps for completeness/appropriateness. Not mandatory, but highly encouraged before sending for READY Assessment. See GSOP-104-1 (Record Creation and Fact Finding). |
| 2 | Launch the Virtual Mentor | Launch from the Action Menu — description and immediate steps auto-populate and analysis starts. |
| 3 | Review the Recommendation | Review suggested improvements as the "human in the loop"; decide whether to act on the feedback and update the text in GDD if needed. Can be run multiple times on a record. |

## 3.0 Root Cause Categorization

**Responsible for this section:** Lead Investigator

| Step | Activity | Related Information |
|---|---|---|
| 1 | Use Root Cause Categorization Tool | **Required.** Run after completing Investigation Details and Root Cause Analysis (RCA) — the tool suggests root cause(s) based on those details. See GSOP-104-3 (Investigation). |
| 2 | Launch the Tool | Launch once deviation/RCA details are complete; details auto-populate and analysis runs. |
| 3 | Review Results | Review as "human in the loop" to decide which suggested root cause(s), if any, belong on the record. |
| 4 | Add, Edit, or Delete Recommended Root Cause(s) | Condition: AI suggestions need changing. Add a different root cause manually, edit an existing result, or delete an incorrect one. |
| 5 | Complete Root Cause Categorization | Once the right root cause(s) are present, complete categorization — anything not deleted transfers automatically to the deviation record's Root Cause object. |

## 4.0 Virtual Mentor for Investigation

**Responsible for this section:** Lead Investigator

| Step | Activity | Related Information |
|---|---|---|
| 1 | Use Virtual Mentor for Investigation | Analyzes description, immediate steps, Quality Risk Evaluation, Investigation Details, CAPA Plan, Recurrence Check Details, and Effectiveness Check Plan for completeness. Not mandatory, but highly encouraged before Review/Approval. See GSOP-104-3 (Investigation). |
| 2 | Launch the Virtual Mentor | Launch from the Action Menu — record fields auto-populate and analysis runs. |
| 3 | Review the Results | Review suggested improvements as "human in the loop"; decide whether to act and update text in GDD as needed. Can be run multiple times. |

## 5.0 Executive Summary Generator (ESG)

**Responsible for this section:** Lead Investigator

| Step | Activity | Related Information |
|---|---|---|
| 1 | Use the ESG | Available after Virtual Mentor for Investigation runs. Uses the deviation details/summary fields to draft an Executive Summary. See GSOP-104-3 (Investigation). |
| 2 | Launch the ESG | Launch from the Action Menu — record fields auto-populate and the generator starts. |
| 3 | Review the Output | Review as "human in the loop" to judge whether the AI draft is appropriate before adding it to the record. Editable in GDD after sending. |
| 4 | Send Draft Executive Summary to Deviation Report | Select "Send to Deviation Record" to populate the summary into the Executive Summary field of the investigation details/summary section. |
| 5 | Changes to the Deviation Record After Executive Summary Generation | Edit the executive summary within the GDD record as needed. |

---
*Source: GSOP-104-3-TL-5, Version 1.0, Effective 14 Jul 2025. Approved 16 May 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
