---
date: 2026-04-06
status:
type: action-items
tags:
  - initiative/supplier-of-focus
---
## Use Material Criticality Only — But Use It Smarter Than Just "Max"

### Why Material Criticality is the right input

- It's the **base-level signal** — Plant and Vendor Crit are just max-rollups of it, so they add no new information
- It gives you the most **granularity** per Supplier, since one Supplier can have hundreds of material-vendor-plant combinations with varying criticality

### Why "max" alone is a bad scoring method

If you just take max(Material Criticality) per Supplier, nearly every supplier with meaningful volume will score HIGH — because they almost certainly supply at least one HIGH material somewhere. That gives you a binary outcome, not a 10-point scale.

### Recommended: Weighted Proportion Score

Score each Supplier based on the **mix** of their material criticalities:

`Score = ( (%HIGH × 3) + (%MEDIUM × 2) + (%LOW × 1) ) / 3 × 10`

This produces a 3.3–10.0 scale where:

- A supplier that is **100% HIGH** → **10.0**
- A supplier that is **100% MEDIUM** → **6.7**
- A supplier that is **100% LOW** → **3.3**
- A supplier that is **26% HIGH, 23% MED, 52% LOW** → **5.8**

This approach:

- ✅ Uses only one criticality column (no double-counting)
- ✅ Differentiates suppliers who supply mostly critical materials from those with only one or two
- ✅ Produces a continuous scale instead of clustering everything at HIGH
- ✅ Reflects _concentration_ of risk, not just _presence_ of risk

### Alternative: You could also factor in **breadth of exposure**

A supplier with 500 HIGH materials across 10 plants is arguably riskier than one with 2 HIGH materials at 1 plant — even though both would score the same on criticality alone. If you want to capture that, you could blend:

- **Criticality score** (the weighted proportion above) — _how critical are the materials?_
- **Footprint score** (number of plants, materials, or unique combos) — _how much exposure do we have?_ [[Footprint Score Methodology]]]
