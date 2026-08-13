---
doc_number: PRD-129872
version: "2.0"
status: Effective
effective_date: 2025-11-04
training_id: GSOP-104-8
tags:
  - nalo/sop
  - nalo/deviation-management
---

# Quality Deviation Management Overview

**Supersedes:** GSOP-104-8, v1
**Associated Standard:** LQS104

## Purpose

Provides an overview of the end-to-end deviation management process within the Global Deviation Database (GDD), governance/oversight expectations, and methods for monitoring deviations across the organization — a consistent global approach aligned to LQS104.

## Scope

A deviation is a departure from approved processes, procedures, instructions, specifications, established standards, GxP, or regulatory requirements — including unexpected occurrences with no procedural response. Applies to all GxP and submission-relevant activities/quality systems within Lilly, plus documentation of external deviations entered into the GDD.

> [!note]
> Investigations of discrepancies, nonconformities, or non-conforming product (as described by some regulations/guidance/industry practice) are synonymous with this deviation management system.

Applies to product/data quality and safety/efficacy information for pharmaceuticals Lilly shares or is legally/regulatorily responsible for, including: associated data & information, GLP, GVP, GCP, GDP, GMP, and regulatory authorization.

## 1.0 High-Level Process Flow

The source PDF includes two process-flow diagrams (Deviation Management Process Overview; Lab Investigation Process Overview) that didn't extract as text — refer to the PDF/QualityDocs directly for the visuals.

## 2.0 Deviation Management Governance and Oversight

### 2.1 Individual Deviation/Lab Investigation Oversight

Area management ensures deviations are identified, reported, investigated, documented, approved, and that required CAPAs are completed. The quality unit ensures deviations are identified/reported appropriately, CAPAs are identified/completed, and effectiveness checks verify actions eliminate root cause(s).

Deviation review is an independent review on major/moderate records requiring in-system review — giving Lead Investigators feedback that the record is properly assessed/classified and that investigation and CAPAs/ECs were created. Deviation approvers (QA and general) provide feedback/oversight and must ensure records are accurate, complete, and LQS104-compliant before approving.

**Table 1 — Roles and Responsibilities** (users must be suitably trained for their tasks):

| Deviation Role | Responsibilities |
|---|---|
| Initiator | Create deviation or lab investigation records in the GDD and complete preliminary information. |
| Fact-Finder | Document all initially available facts about the deviation — times, locations, impacted areas, objects, batches, and products. |
| READY Assessor | Perform assessments including the quality risk assessment and RPN-based classification. Must be a quality representative for all deviations. |
| Lead Investigator | Complete all required investigation activities including root cause analysis; create/assign child records (child investigations, user tasks); ensure they're complete; complete the investigation summary; create associated CAPAs/ECs. |
| Reviewer | Review deviation records for completeness/accuracy after investigation; give feedback and determine if revisions are required. |
| Approver | Approve records via electronic signature, confirming they've been reviewed for accuracy/completeness, agreeing with conclusions, and confirming appropriate CAPAs were created. |
| User Task Owner | Complete the assigned task per its instructions by the due date. |
| Child Investigation Owner | Complete the assigned Child Investigation per instructions by its due date. |
| CAPA Owner | Implement and complete the assigned CAPA; document outcomes/results. |
| EC Owner | Execute assigned Effectiveness Checks per instructions/acceptance criteria; assign a verdict based on results. |
| EC Verifier | Review and confirm execution/outcomes of an EC. |
| One-Time Approver | Approve Private User Tasks or Private Child Investigations without reviewing/approving the related deviation or lab investigation record. |
| Quality Alert Initiator | Create/document Quality Alerts and Addendums so alerts reach recipients. |
| Quality Alert Reviewer | Review Quality Alerts/Addendums before sending; assign a verdict for approval to send. |
| Private User Task Owner | Complete the assigned Private task per instructions by the due date. |
| Private Investigator | Complete the assigned Private Investigation per instructions by the due date. |
| External Party | Complete assigned tasks as a non-Lilly user via a temporary external link. |
| CAPA Verifier | Review and confirm actions/outcomes of an implemented CAPA. |
| Lab Investigator | Complete all required investigation activities (Phase I, Phase II, Full-Scale) including root cause analysis; create/assign child records (test plans, child investigations, user tasks); complete the investigation summary. |
| Lab Investigation Quality | Complete tasks within a Laboratory Investigation record as a QA representative. |
| Test Plan Owner | Complete Test Plans per Task instructions by the due date. |
| Impact Assessment Owner | Complete the assigned Impact Assessment per instructions by the due date. |

### 2.2 Deviation Local Governance

Local sites/areas must apply all requirements from this procedure and LQS104. Sites are responsible for local governance that handles feedback from global governance, monitors their own deviations, identifies systemic issues, and implements actions to keep the deviation management system suitable, adequate, and effective — and to identify improvement opportunities.

### 2.3 Deviation Global Governance

The Deviation Global Process Owner (GPO) owns the deviation management process and system performance — managing GDD changes, global document reviews/updates, monitoring overall system performance, and escalating issues/opportunities to management.

## 3.0 Deviation Management Process Overview

### 3.1 Deviation Identification

All employees must watch for potential-deviation indicators; if suspected, take an immediate response to prevent further impact where possible, then create a deviation record documenting the departure and report to immediate supervision. If a user lacks GDD access, contact the appropriate area representative to create the record.

The Quality Incident module offers two other deviation-identification workflows:
- **Intracompany Issue (ICI)** — manages issues discovered between sites requiring assessment on whether a deviation is needed; create (or relate to an existing) deviation if the assessor determines investigation is required. See [[GSOP-104-1-TL-5]].
- **Trend Triage** — for a suspected trend/emerging theme requiring quality assessment on whether investigation is needed; create (or relate to an existing) deviation if required. See [[GSOP-104-7-TL-1 Trend Triage]].

### 3.2 Deviation Notification

The READY Assessor (a QA member) must be notified of deviations — typically by assigning the record to them in the GDD. See GSOP-101-3 (Quality Notification to Management) for events qualifying for further escalation. Immediate regulatory-authority reporting of quality issues follows LQS112 (Regulatory Authority Communication and Inspections).

### 3.3 Record Creation and Fact Finding

Deviations must be created in the GDD within 1 business day of discovery, with a unique number, defined problem statement, and event information. Fact Finding identifies scope — batch impact, organization impact, clinical study impact, assets, materials — and evaluates impact on patient/research safety, product quality, safety/efficacy data, and regulatory information. Batch/product inclusion or exclusion from scope must be justified in the record. Include all known facts before assessment to inform risk understanding and classification. The Fact Finder may use the AI Virtual Mentor for feedback on the description/immediate actions. For batch-related deviations, the deviation number must be recorded in the batch production record or equivalent. See [[GSOP-104-1 Record Creation and Fact Finding]].

### 3.4 READY Assessment

READY (Risk Evaluation, Actionable Direction, and Yield Results) Assessment applies to deviations (not Lab Investigations). The READY Assessor considers all risk elements and provides direction/guidance for the Lead Investigator (e.g., root cause tools, suggested corrections), confirms processing/materials/objects in scope remain suitable for continued use, reviews involved/impacted batches, and assesses whether escalation or market-product impact applies.

Classification uses the Risk Priority Number (RPN) — scored from severity, detectability, and occurrence questions — into **Major**, **Moderate**, or **Minor** (Major = highest risk/severity). The READY Assessor may override the RPN classification with justification.

Deviations may be cancelled up to READY Assessment, but only by Quality, with justification.

### 3.5 Investigation

Purpose: determine/address root cause, identify undetected additional impacts, evaluate final quality/regulatory impact, and define CAPA/EC — with effort/formality/documentation commensurate with deviation criticality.

- Apply a risk-based, documented approach to root cause investigation.
- Investigate quality system elements (process/procedural/system-based errors) first; personnel-performance root cause requires justification. If true root cause can't be determined, identify the most likely one(s).
- Identify and implement appropriate CAPA(s)/ECs.

Impact must be investigated/documented considering: patient/research subject safety; product/data quality; safety/efficacy information quality & integrity; regulatory information/marketing authorization compliance. For GMP/Manufacturing sites, also consider: stability impact (incl. need for additional studies), product/process monitoring results, process/systems qualification/validation state, and whether the deviation is systemic (control strategy effectiveness, quality system adequacy).

Major/moderate deviations require a **recurrence check** (same area, same description/root cause) after investigation — minor deviations don't need it since an occurrence check already runs at READY Assessment. Repeat occurrence raises the RPN score and may elevate classification.

AI tools available to the Lead Investigator (all require human-in-the-loop review):
- **Root Cause Categorization** — suggests root cause(s) from the investigation report.
- **Virtual Mentor for Investigation** — in-system feedback/suggestions on investigation reports.
- **Executive Summary Generator** — drafts an executive summary. See [[GSOP-104-3-TL-5 AI for Deviations]].

### 3.6 Review and Approval

Where mandatory, an independent **deviation review** happens after investigation and before approval, confirming appropriate investigation, correct root cause, and appropriate CAPAs/ECs — captured in GDD via reviewer feedback to the Lead Investigator on whether updates are needed.

**Approval** is the final step — minimum numbers/levels of approvers (business and quality) depend on classification and area. After approval, CAPA implementation begins or the record closes; QA must approve before release of impacted batches/materials. Internal deviations: approve/complete within 30 calendar days of discovery. External: 45 calendar days. Missing the window requires an approved extension request before the due date (Quality must approve at minimum); if approved/completed late without one, an amendment addressing the missed deadline/risk is required.

Minor post-approval information changes go through the **Amendment process** — any content modification after approval (batch disposition, planned CAPA, ECs) must be re-approved by the original approvers or equivalents. Approved/completed records cannot be reopened.

### 3.7 CAPA and EC

**Corrections** mitigate a deviation event without addressing root cause; created within the deviation record, implemented before or after approval.

**CAPAs** address identified root cause(s) — required (or an existing CAPA linked) for major/moderate deviations, with the root cause record linked to the CAPA; implemented after approval. No-CAPA-needed determinations on major/moderate deviations require justification.

For medical devices/device constituents of combination products, CAPAs require verification that the action doesn't adversely affect regulatory requirements or device safety/performance — evaluated before CAPA plan approval and during effectiveness verification.

CAPA/Corrections must not be investigational; scope-of-impact assessments needed to assign actions must complete before investigation approval.

Every CAPA requires a linked **EC** to verify root cause was addressed and recurrence is prevented. Quality must verify CAPA implementation for all; EC verification by Quality is required for major/moderate deviations. Ineffective/inconclusive ECs require appropriate follow-up action.

### 3.8 External Deviations

Created/managed by external parties (including external labs) and entered into GDD for tracking/investigation per established agreements. Must be approved/completed within 45 days of discovery. The External Collaboration feature lets external parties contribute directly via a secure link.

### 3.9 Privacy

Deviation data requiring privacy goes in a Private User Task or Private Investigation — e.g., personnel name/ID tied to a performance issue; cases where personnel performance is a significant contributing root cause; sensitive personal information; or investigation details identifying a personnel performance/human-error issue, or as instructed by management.

### 3.10 Actions in Response to Nonconforming Product Detected Before Delivery

Disposition nonconforming product by: eliminating the nonconformity, precluding its original intended use, or authorizing use/release/acceptance under concession. Concession acceptance requires justification, approval, and regulatory compliance — with records of the acceptance and the authorizing person maintained.

## 4.0 Laboratory Investigation Process Overview

### 4.1 Laboratory Investigations

Address Out of Specification (OOS), Out of Trend (OOT), or Out of Expected Result (OOE) results via a separate GDD workflow covering Phase I, Phase II, and Full-Scale (deviation) investigations. Any atypical result (OOS/OOT/OOE) must be investigated; confirmed OOS results or significant negative trends affecting marketed batches should be reported to relevant competent authorities, considering possible market-batch impact.

Lab Investigations must be approved within 30 calendar days of discovery. A related deviation record uses the same discovery date and also gets 30 days to complete. Post-approval changes follow the same Amendment process described in 3.6 (re-approval by original approvers; approved/completed records can't be reopened).

### 4.2 Laboratory Investigation Identification

Analysts must check sample data against test specifications before discarding preparations; preparations must not be discarded before second-person verification. Once an OOS/OOT/OOE is identified (post second-person verification), initiate a lab investigation and notify area supervision and quality.

For chemical methods, investigate as OOS if: one or more replicates are outside specification while the overall result is within spec, or any OOS result occurs even when method suitability/acceptance criteria weren't met. For animal/biological/microbiological methods, OOS/OOE definitions come from the specific analytical test method.

Log the OOS/OOT/OOE result (electronic or hardcopy) with: test, product/material, batch number, Lab Investigation number, and date. For OOS/OOT stability results, immediately notify the person responsible for stability study oversight, who determines study status and recommends to the site quality leader whether to continue/discontinue the lab and/or deviation investigation.

### 4.3 Phase I Lab Investigation

Performed to identify any obvious/immediate assignable cause once an OOS/OOT/atypical result is identified. If no obvious cause is found, proceed to Phase II.

### 4.4 Phase II Lab Investigation

Conducted when Phase I can't immediately identify an assignable cause. Lab area supervision ensures completion with both a Quality approver and a technical approver involved; the technical approver ensures documentation and scientific rationale throughout. Investigational testing may confirm or disprove a hypothesis. If an assignable cause is found, original results are invalidated and a repeat test is performed as appropriate.

### 4.5 Full-Scale Investigation (Deviation)

If no assignable cause is found in the Lab Investigation, extend into manufacturing via a deviation (a deviation may also start any time before the lab investigation completes, if warranted). If an assignable cause is found during the deviation, both records are completed/approved; if not, a retest may confirm the OOS/OOT result. If the original sample was consumed or its integrity compromised, a resample may support a repeat test or retest. Quality and technical approvers must approve the full-scale investigation at the deviation review step within the Lab Investigation record.

### 4.6 Test Plans

Support lab and/or deviation investigations.
- **Retest** of the original sample: only when no assignable cause explains the result via a deviation investigation; permitted once per sample — any departure requires site quality leader approval.
- **Resample**: only when the original sample was consumed or has a proven integrity issue; must use the same sampling method as the original unless that method was determined incorrect (document/justify any difference).

## 5.0 Deviation/Laboratory Investigation Monitoring

Two elements:
1. **Emerging Themes** — data review to spot unexpected patterns/anomalies in deviation data, surfacing hidden/unknown risks from occurrence checks, recurrence checks, and ad hoc monitoring.
2. **Performance Monitoring** — data analysis against predetermined parameters to ensure the deviation system stays suitable/adequate/effective and to find improvement opportunities; reported/reviewed per site/area/function and globally.

### 5.1 Emerging Themes

Monitored via:
- **Occurrence Checks** — run at READY Assessment on all deviations to flag repeat-instance risk; feeds the RPN tool and may elevate classification.
- **Recurrence Checks** — run on moderate/major deviations, reporting how many times a similar deviation (same area, same root cause) has occurred. If existing deviation/CAPA scope already addresses the recurrence, no further investigation is needed; otherwise a Quality Incident can assess whether a new deviation is required.

Additional monitoring (e.g., Periodic Deviation Monitoring Report, ad hoc queries) may surface emerging themes periodically or as needed — any anomaly found this way goes through a Quality Incident (QI) record assessment to determine if a new deviation is required. See [[GSOP-104-7-TL-1 Trend Triage]].

### 5.2 Performance Monitoring

Assessed via the Periodic Deviation Monitoring Report (PDMR). Local governance establishes performance monitoring; sites/functions periodically analyze metrics/data and document suspected trends in the Quality Incident module to assess if further investigation is warranted. Global-level monitoring additionally identifies trends/process improvements and drives continuous improvement of the deviation management system.

## Appendix A — Related Procedures

| Procedure | Intent |
|---|---|
| GSOP-104-1, Record Creation & Fact Finding | Creating deviation records in GDD and performing Fact Finding, including adding impacted items (e.g., batches). See [[GSOP-104-1 Record Creation and Fact Finding]]. |
| GSOP-104-2, READY Assessment | Identifying risk and classifying deviations via the RPN tool. |
| GSOP-104-3, Investigation | Conducting deviation investigations, including root cause identification, CAPAs/ECs, and AI tool use. |
| GSOP-104-4, Review and Approval of Deviations | Requirements/process for deviation reviews and approvals. |
| GSOP-104-5, External Deviations | Requirements/execution for deviations involving external parties, including External Collaboration. |
| GSOP-104-6, Laboratory Investigation | Identifying and conducting Lab Investigations (Phase I, Phase II, Full-Scale). |
| GSOP-104-7, Deviation Monitoring Program | Monitoring GDD events to assess the health of the Deviation Management Program. See [[GSOP-104-7-TL-1 Trend Triage]]. |
| GSOP-101-1, CAPA and Effectiveness Checks | Creating CAPAs/ECs and the CAPA implementation/EC execution/verification process. |

---
*Source: GSOP-104-8, Version 2.0, Effective 04 Nov 2025. Approved 24 Oct 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
