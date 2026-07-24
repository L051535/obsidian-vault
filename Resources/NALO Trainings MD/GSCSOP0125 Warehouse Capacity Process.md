---
doc_number: PRD-93179
version: "3.0"
status: Effective
effective_date: 2026-01-12
training_id: GSCSOP0125
tags:
  - nalo/sop
  - nalo/warehouse-capacity
---

# Warehouse Capacity Process

**Supersedes:** SCM-SOP-GSC-00006 Rev. 02
**Areas Impacted:** Warehousing & Logistics for Lilly API/SFG/FG Manufacturing Sites, SFP, Global Planning

## Purpose

Describes the process for assessing and managing warehouse capacity across Lilly manufacturing sites and affiliated organizations, including the periodic evaluation used to determine warehouse capacity status per site and identify mitigation measures: low-capital initiatives (Local Capital Plan), internal warehouse expansion, or external warehouse utilization.

For external warehouse operations, business/quality requirements are defined in **GQS-307** (GMP Service Providers and Consultants) and **LQS-102** (Quality Management of Collaborations with External Parties) — governing qualification, approval, and ongoing management of third-party (non-Lilly managed) warehouses.

> [!note]
> When storage capacity constraints are identified, the manufacturing site notifies **Global Supply Chain (GSC)** and **Global Logistics (GLX)** for a joint review. Approval of any external warehouse project requires review/endorsement by **Global Logistics Senior Leadership** with **Manufacturing Leadership**. Capacity needs and mitigation strategies feed into the annual Strategic Plan and Business Plan processes.

Process scope: starts with periodic warehouse capacity evaluation, ends when projects scale warehouse capacity to the defined level.

## Requirements

- Most recent maximum storage capacity data, strategic/business plan info, and WMS data are sourced from manufacturing sites.
- Pertains to: Distribution, Storage, Warehousing, Packaging and Labeling.
- Pertains to: API Starting Material, Intermediates, APIs, Bulk Drug Products, Drug Products, Finished Products, Medical Devices (and device components), Drug/Device Combination Products.

## Process Flow (Figure 1)

`Collect SP or BP forecast data → Capacity Evaluation with Global Capacity Tool → Do you have enough space? → [Yes: STOP] / [No: Can you implement low capital project? → Yes: Implement VMI, SS reduction, Relayout, pallet density optimization → back to Capacity Evaluation / No: Evaluate make or buy solution with capacity handbook → Implement the decided solution → STOP]`

Applied during both SP (Strategic Plan) and BP (Business Plan) cycles.

## 5.1 Governance

- Lilly is committed to maintaining sufficient warehouse capacity for efficient inventory storage, operational needs, and future growth across all manufacturing sites.
- This annual process, led by **GSC**, begins with forecasts from BP/SP volume sessions, used in the **Global Warehouse Capacity Forecast** process to estimate required storage capacity levels.
- Comprehensive evaluation occurs at both SP and BP stages, at minimum.
- GSC engages site logistics SMEs at process initiation; GSC loads/updates data but must align with each site on inclusions/exceptions every cycle.

## 5.2 Collect SP and BP Data

- SP/BP data is processed and provided to Global Logistics by GSC's Strategic Facility Planning (SFP) and/or Global Planning.
- Global Logistics converts this data into the format required by the **Warehouse Capacity Tool (WCT)**.

## 5.3 Capacity Evaluation

- GLX determines warehouse occupancy level for each site across the analysis period (SP or BP horizon), calculating initial occupancy to **85% utilization**.

> [!note]
> Monitoring and adjusting a site's utilization is a **shared responsibility** between the site and GLX.

- WCT integrates: SP/BP data, historical WMS data (12-month average occupancy from monthly inventory snapshots), and internal/external warehouse capacity data per site, plus volume projections/demand forecasts aligned with GS&OP processes.

## 5.4 Space Evaluation

- Occupancy data (pallets/handling units) per warehouse area is compared against defined capacity; areas exceeding/nearing thresholds are flagged.
- Local site SME and GSC representative align on:

**Space Evaluation Priorities:**
- **Baseline Validation** — confirm baseline reflects prior period's WMS occupancy.
- **Site Optimization** — evaluate on-site storage availability/constraints for reallocating materials (storage balancing).
- **Forecast Alignment** — align site leadership and GSC on forecasted occupancy levels.

- If occupancy is within acceptable limits per **STG-08601** (Manufacturing Warehouse Strategy for API/Drug Substance and Drug Product), evaluation concludes.
- If additional capacity is required, GLX and Site Manufacturing Leadership jointly develop an expansion plan.
- If the site elects not to prioritize expansion, **Attachment D – Exception to Business Process Request Form** must be completed, signed by the **Site Head** and **Global Logistics AVP**.

## 5.5 Low Capital Project

First potential solution after Space Evaluation alignment: internal expansion via a low-capital project, managed as a formal project within the local capital plan process.

**Examples:** installation of new freezers for APIs, shelving remodulation, re-layout of manual warehouses, minor inventory/buffer adjustments.

If insufficient, proceed to a "make versus buy" evaluation.

## 5.6–5.7 Evaluation: Make versus Buy

- Confirmed need tied to new production lines/site expansion → formal project under PMO principles/oversight.
- GLX recommends a course of action based on:
  - Duration and go-live requirements
  - Material matrix considerations
  - Planned days on hand (DOH) for inventory eligible for external storage

## 5.8 Risk Assessment for External Warehouse Operations and 3PL Selection

- External operations introduce risk: natural hazards, cost variability, contract terms, distance to manufacturing sites.
- 3PL engagement adds: specification control, service reliability, engineering standards compliance (e.g., FM Global).
- 3PL location selection requires holistic review of risk, timeline, warehouse size, revenue protection, volume projections, operational requirements.
- Final decisions governed by the **GSC Resilience Team** with the **Global Risk Team**.

## 5.9 Duration and Go-Live

Three key parameters:
1. **Duration of need** — for temporary/short-term needs, prefer external warehouse space (even deviating from material matrix) since CAPEX for short-term space isn't sustainable.
2. **Material matrix** (per STG-08601) — primary guide for internal vs. external storage under standard conditions.
3. **Internal buffer impact** — typically 3–5 days for raw materials; if it drops below 3 days during expansion, internal adjustment is needed, generally requiring a corresponding external capacity adjustment (hybrid storage strategy).

GLX issues a recommendation to the **GLX Lead Team**; if approved, it goes to the **Manufacturing Leadership Team (MLT)** for final sourcing decision approval.

## 5.10 MLT Sourcing Decisions — Two-Gate Framework

- **Gate 1:** Evaluates internal vs. external manufacturing options via business case analysis (cost, capability, product-specific data, boundary conditions/assumptions). Internal recommendations require MLT approval to insource; external recommendations proceed to Gate 2. Each Gate 1 request must include boundary conditions and should be reviewed in-person at MLT.
- **Gate 2:** Finalizes the sourcing decision — specific Lilly site or external partner/supply site and agreement parameters. Includes business case analysis, diligence assessment (engagement risk, financial, quality, health, safety, environmental, IP), negotiation of supply agreement parameters, and implementation requirements (expenses, capital, resources, timeline, ongoing support/oversight).

## 5.11 Implement the Solution

- Requirements must be defined/documented before initiation: pallet positions, storage condition specs, automation level, fire protection systems, transportation needs, throughput requirements.
- **Internal expansion:**
  - Global capital project → follow Global Facilities Delivery (GFD) procedures.
  - Local capital project → follow local site procedures.
- **External expansion:** Global Logistics + Global Procurement initiate an RFI/P process among existing partners and qualified market providers, aligned with Global Procurement's External Vendor Strategy. Project team ranks candidates, considering Total Cost of Acquisition (TCA) over the engagement duration.
- TCA results determine required approval levels (see Procurement Playbook, PR-9487940).
- Global Procurement notifies the selected partner and manages the contract.
- The using organization/site initiates change control and executes readiness/compliance activities for the new space.

## Terms and Acronyms

| Acronym | Meaning |
|---|---|
| AGV | Automated Guided Vehicle |
| AMR | Autonomous Mobile Robots |
| API | Active Product Ingredient |
| ASRS | Automated Storage and Retrieval System |
| BP | Business Plan |
| CDA | Confidential Disclosure Agreement |
| DC | Distribution Center |
| DOH | Days on Hand |
| DDMRP | Demand Driven Material Requirements Planning |
| FG | Finished Goods |
| FRAP | Financial Responsibility and Authorization Policy |
| GQS | Global Quality Standards |
| GSC | Global Supply Chain |
| HSE | Health and Safety/Environmental |
| HU | Handling Unit (Pallet, Bottles, Trays, Container...) |
| LSP | Logistic Service Provider (also 3PL) |
| MSDS | Material Safety Data Sheet |
| QA | Quality Assurance |
| RFP | Request for Proposal |
| SC | Supply Chain |
| SFG | Semi-Finished Goods |
| SP | Strategic Plan |
| 3PL | Third Party Logistic |
| WIP | Work In Process |

## Related Documents

| Document Number | Title |
|---|---|
| EBP-0130 | Warehouse Facility, Engineering Best Practices |
| GQS-202 | Facilities |
| GQS-307 | GMP Service Providers and Consultants |
| LQS-102 | Quality Management of Collaborations with External Parties |
| GLX-RES-0003 | Pharmaceutical Warehouse Capacity Management Handbook |
| STG-08601 | Manufacturing Warehouse Strategy for API/Drug Substance and Drug Product |
| PR-9487940 | Procurement Playbook |
| GSCSOP0125-TOOL-01 | External Warehouse Implementation Process |
| GSCSOP0125-TOOL-02 | Warehouse Dimensioning |
| GSCSOP0125-TOOL-03 | Warehouse Suggested Related Metrics |
| GSCSOP0125-FORM-01 | Exception to Business Process Request Form |

## Reasons for Revision (current version)

1. Separation of warehouse strategies and warehouse capacity processes throughout.
2. Updated title to remove "Strategy".
3. Updated to new naming convention/template per GSCSOP0001.
4. Created Tools and Forms from previous attachments within the SOP.

---
*Source: GSCSOP0125, Version 3.0, Effective 12 Jan 2026. Approved 16 Dec 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
