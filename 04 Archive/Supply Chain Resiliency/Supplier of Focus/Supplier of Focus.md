---
date: 2026-04-07
status: closed
type: index
tags:
  - initiative/supplier-of-focus
---
## Overview

> [!abstract] Purpose
> Building the "Supplier of Focus" risk-scoring model — a composite 0–10 score across six weighted factors (spend, material group criticality, product criticality, external/geopolitical risk, part concentration, supplier criticality) used to classify Lilly's ~830 suppliers as HIGH/MEDIUM/LOW risk.

| Field            | Value      |     |
| ---------------- | ---------- | --- |
| **Owner**        | —          |     |
| **Stakeholders** | John Hamilton (procurement), Greg Magnussen (product lifecycle management), Paul |     |
| **Started**      | 2026-04-06 |     |

---

## Key Notes

- [[Supplier Risk Assessment Instructions]] — full replication instructions for the v5 scoring model (six weighted factors, live Excel formulas)
- [[New Supplier Criticality Approach]] — recommended weighted-proportion scoring method for the Supplier Criticality factor
- [[Footprint Score Methodology]] — percentile-rank approach for scoring how embedded a supplier is (plants served × materials supplied)
- [[Data Source Info]] — raw ZS163n data source, old-vs-new risk factor list, per-factor assumptions/filters

---

## Active Tasks

```tasks
not done
path includes Archive/Supply Chain Resiliency/Supplier of Focus
```

---

## Notes & Meetings

| Note | Date | Summary |
| ---- | ---- | ------- |
| [[Data Source Info]] | 2026-04-06 | Consolidated 2024 risk factors down to 8 for 2026: spend, material group, products, geopolitical (ICRG), part concentration, supplier criticality, single-sourced plant/material combos, footprint risk |
| [[Footprint Score Methodology]] | 2026-04-06 | Recommends percentile-rank scoring (not raw counts) since supplier footprint distribution is heavily skewed |
| [[New Supplier Criticality Approach]] | 2026-04-06 | Recommends a weighted-proportion score (%HIGH×3 + %MED×2 + %LOW×1)/3×10 instead of a simple max, to avoid every supplier scoring HIGH |
| [[Supplier Risk Assessment Instructions]] | 2026-04-15 | v5 replication instructions: 830 suppliers, 6 weighted factors, live-formula Excel output with editable weights |
| [[Supplier of Focus v6 (NEXT STEPS)]] | 2026-05-08 | Brief next-steps note — model near-complete, follow-up items pending |

---

## Decisions & Context

> [!info]- Background
> The 2026 model deliberately dropped several 2024 risk factors (the prior year's own Supplier of Focus score, and Supplier Manufacturing Site Risk) as unmaintainable or double-counting, replacing site-risk with an Everstream-sourced measure instead. Composite score weights (v5 default: Spend 15%, Material Group 10%, Product 25%, External Risk 20%, Part Concentration 15%, Supplier Criticality 15%) are stored on a live-formula Weights tab so they can be adjusted without rebuilding the model.

---

## Related

- **Parent:** [[Supply Chain Resiliency Index]]
- **Upstream dependencies:** Corporate Finance spend data, Everstream (external risk), SAP ZS163n
- **Downstream / outputs:** —
- **See also:** [[Everstream]] (feeds the external/geopolitical risk factor)

---
