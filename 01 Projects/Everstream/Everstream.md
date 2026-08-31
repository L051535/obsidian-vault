---
date: 2026-04-07
status: closed
type: index
tags:
  - initiative/everstream
---
## Overview

> [!abstract] Purpose
> Standing up Everstream as Lilly's external risk-intelligence layer — facility/supplier data pipeline, incident triage views, training rollout, and integration strategy alongside Project44 (shipment visibility), SAP TM, and Kinaxis.

| Field            | Value      |     |
| ---------------- | ---------- | --- |
| **Owner**        | Brooke Kendzicky |     |
| **Stakeholders** | Steven Chen, Arturo, Philippe Perrin, Mar Gimeno, Jose Watlington, Laurel |     |
| **Started**      | 2026-04-01 |     |

---

## Key Notes

- [[Everstream ZS163n Filters]] — User Status filters used on the base ZS163n report (SAP Export, not SHARP)
- [[Everstream Views Development]] — the 7 saved triage views (New/Assessing/Resolved × High/Moderate severity, All Incidents)
- [[Everstream Changes for Arturo]] — technical changelog of manual data-pipeline changes (facility groups, user status filters, material flow classification logic)
- [[Material Flow Generation]] — build instructions for generating the Material Flow Data sheet from BOM + ZS163n data
- [[Facility Data Documentation]] — open concern about tracking what changed in the data and continuity once Alex moves on

---

## Active Tasks

```tasks
not done
path includes Archive/Supply Chain Resiliency/Everstream
```

---

## Notes & Meetings

| Note | Date | Summary |
| ---- | ---- | ------- |
| [[Questions for Tomorrow Meeting 4-7-2026]] | 2026-04-07 | Stakeholder prep: GLX Everstream training questions (Jose Watlington), dashboard/workflow views per group, triaging status/action plan model, incident timeline scope |
| [[Meeting 4-21-26]] | 2026-04-21 | Everstream webhook/API setup contacts; training scheduling ("make or break" session + office hours) |
| [[Everstream Training Roll-out]] | 2026-04-22 | Teams in scope for training: Logistics, Transportation, TPRM, Procurement, Global Materials, US/IBU Demand Planning, Parenteral/API/Dry Planning |
| [[4-28-2026 Meeting]] | 2026-04-28 | Facility data vs. Material/Material Flow data are separate databases with different update/ownership models |
| [[Brooke Future Everstream Plans]] | 2026-04-30 | Roadmap: stabilize facility dataset first, defer custom logistics risk scoring, treat manual data maintenance as its own scope item, "empower don't centralize" operating model |
| [[Everstream May 5 Questions]] | 2026-05-05 | Open questions: mass upload of internal risk scores, saved "My network" user profiles |
| [[Training & Access]] | 2026-05-20 | Dashboard/workflow views per stakeholder group, playbooks/action plan ownership model, incident timeline behavior (active/inactive list rules) |
| [[P44 & Everstream Integration]] | 2026-05-26 | Everstream (risk intelligence) vs. Project44 (real-time shipment visibility) role division; integration is limited/indirect; data ownership and use-case-driven design flagged as the real gaps |
| [[Everstream Lilly In-Person Meeting]] | 2026-05-28 | Everstream vs. P44/SAP TM/Kinaxis positioning; biggest gap identified is node visibility without lane (route) visibility |

---

## Decisions & Context

> [!info]- Background
> Recurring theme across the project: Everstream's value depends more on data governance (who owns manual facility entries, refresh cadence, integration architecture) than on the tool itself. The team repeatedly chose to stabilize/document before expanding scope, and to defer bespoke logistics risk scoring until a clear use case emerges. The "nodes vs. lanes" gap (strong on sites/suppliers, weak on route/lane visibility) was identified as the single highest-value next investment.

---

## Related

- **Parent:** [[Supply Chain Resiliency Index]]
- **Upstream dependencies:** SAP ZS163n export, corporate facility master data
- **Downstream / outputs:** [[Supplier of Focus]] (external/geopolitical risk factor), [[Iran Conflict]] (connected-incident example)
- **See also:** [[Optimus]] (shares Arturo as a data contact)

---
