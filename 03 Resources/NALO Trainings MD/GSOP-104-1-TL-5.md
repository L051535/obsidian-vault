---
doc_number: TL-340514
version: "2.0"
status: Effective
effective_date: 2026-01-09
training_id: GSOP-104-1-TL-5
tags:
  - nalo/sop
  - nalo/deviation-management
---

# Intracompany Issues

**Supersedes:** GSOP-104-1-TL-5, v1
**Associated Procedure:** [[GSOP-104-1 Record Creation and Fact Finding|GSOP-104-1]]

## Purpose

This global required tool describes the process and management of Intracompany Issues (ICIs) within the Quality Incident module in the Global Deviation Database (GDD).

## Scope

- **In Scope:** Issues between sites that require an assessment to determine whether a deviation is needed to investigate them.
- **Out of Scope:** Market complaints or issues discovered during distribution of finished products/devices. If the issue meets the definition of a deviation or impacts batches, the deviation process must be used instead — ICI is not for investigation, batch impact management, root cause, or CAPA.

## 1.0 Overview of the ICI Process

ICIs are assessments between sites/functions within the enterprise that triage whether an issue requires further investigation via a deviation record. The site/function personnel who identify the ICI create the record and assign representatives from the other site/area involved to assess it and determine next steps. If a deviation is identified, the standard deviation process is followed to investigate and identify root cause.

## 2.0 Create and Assign the ICI

| Step | Activity | Related Information | Responsible |
|---|---|---|---|
| 1 | Create the ICI | Created by the Organization Level 1 that discovered the issue; include supporting pictures/attachments. Must be created within 1 working day of identifying the issue. ICI records get an automatic 2-calendar-day due date from creation, to keep deviation initiation timely when one is required. | Initiator |
| 2 | Batch is Impacted | Condition: an ICI already exists. There's no batch object on an ICI — to flag a potential deviation in SAP, the deviation module must be used instead. Complete the missing ICI info and send for assessment; request the assessor complete it with a new deviation if required per Section 3.0, Step 4. | Initiator |
| 3 | Identify Areas/Sites Involved | Identify the Triaging Organizational Level 1 (the site/function responsible for the ICI assessment); include additional org levels of the triage site/area as needed. | Initiator |
| 4 | Assign the ICI Team | Assign the team members (e.g., triage) who will assess whether a deviation is required. | Initiator |
| 5 | Review the ICI Record | Review for completeness/accuracy; update as needed, including supporting pictures/attachments. | Owner |

## 3.0 Perform the ICI Assessments

| Step | Activity | Related Information | Responsible |
|---|---|---|---|
| 1–2 | Complete the ICI Assessment | Review the record and provide the assessment outcome/rationale confirming whether the issue meets the definition of a deviation; select whether a quality event is required. | Assessor |
| 3 | Request Additional Information | Condition: the record is incomplete or more information is needed. Mark that additional information is required so the record returns to the Owner to complete/provide it. | Assessor, Owner |
| — | Complete the Assessment with No Quality Event Required | Condition: no quality event is required. Confirm the assessment is complete and the issue doesn't meet the deviation definition; the record returns to the Owner to review and either send back to the assessor or close it. | Assessor, Owner |
| — | Complete the Assessment with a Deviation Required | Condition: a deviation is required. Confirm the assessment, that the issue meets the deviation definition, and indicate whether a deviation is already in progress to relate to the ICI (relate it if so). If no deviation is in progress, one is created automatically and the Owner is assigned as Fact Finder. See GSOP-104-1 (Record Creation and Fact Finding) and GSOP-104-1-TL-2 (Multi-Track Deviations). | Assessor, Owner |

---
*Source: GSOP-104-1-TL-5, Version 2.0, Effective 09 Jan 2026. Approved 10 Nov 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
