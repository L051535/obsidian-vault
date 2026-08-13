---
date: 2026-04-07
status: closed
type: index
tags:
  - initiative/optimus
---
## Overview

> [!abstract] Purpose
> Modeling accurate production capacity ("net run rate") per site by product, accounting for changeover time between dosage/strength combinations — and separately, resolving demand data down to count/strength-level splits so it can be matched against that capacity.

| Field            | Value      |     |
| ---------------- | ---------- | --- |
| **Owner**        | —          |     |
| **Stakeholders** | Arturo, Pablo |     |
| **Started**      | 2026-03-25 |     |

---

## Key Notes

- [[Planning Setup & Prod.]] — Excalidraw drawing referenced for net-run-rate/changeover setup logic
- `[AIO] AllinOne Detailed Scheduling` and `[AllinOne] AllinOne - APO` — Power BI reports referenced for resources and count/strength splits (see [[Data Alignment Arturo]] for links)

---

## Active Tasks

```tasks
not done
path includes Archive/Supply Chain Resiliency/Optimus
```

---

## Notes & Meetings

| Note | Date | Summary |
| ---- | ---- | ------- |
| [[Pablo Demand & Production Data Working Session (3-25-2026)]] | 2026-03-25 | Demand data is aggregated (no strength/count splits); plan to use prior-year actual sales to derive count splits per market, still need strength splits |
| [[Data Alignment Arturo]] | 2026-04-08 | Arturo's by-line production rate approach factors in changeover time based on dosage/strength/country combinations, averaged over a year of production plan data to get a "net run rate" |

---

## Decisions & Context

> [!info]- Background
> Core modeling challenge: even for the same product, changeover/setup time varies by count, strength, and destination country — so a single flat production rate understates real capacity constraints. Arturo's approach averages a year of actual production-plan data (not engineering specs) per site to derive a realistic net run rate.

---

## Related

- **Parent:** [[Supply Chain Resiliency Index]]
- **Upstream dependencies:** Demand data granularity (count/strength splits)
- **Downstream / outputs:** —
- **See also:** [[NALO PDC Index]] (related capacity-modeling theme, different scope)

---
