---
date: 2026-04-06
status:
type:
tags:
---
### Material Criticality

**Scope:** One material + one MP Vendor

This is the base-level assessment. It answers: _"How critical is this specific material when sourced from this specific MP Vendor?"_ The same material can have different criticality ratings depending on which vendor supplies it (e.g., a chemical sourced from a sole-source vendor may be HIGH, while the same chemical from an alternate vendor is LOW).

**Values:** HIGH, MEDIUM, LOW, EXTRNL, NONE

---

### Plant Criticality

**Scope:** One MP Vendor + one Plant **Rule:** **max(Material Criticality)** across all materials that MP Vendor supplies to that plant

It answers: _"How critical is this vendor to this specific plant?"_ If a vendor supplies 30 materials to a plant and 29 are LOW but one is HIGH, the Plant Criticality is HIGH — because the plant depends on that vendor for at least one critical material.

**Confirmed at 97–99.8%** across 461K rows. The small number of exceptions appear to be data quality lags.

---

### Vendor Criticality (RawDataEx-INAC-PCodeJ:J)

**Scope:** One MP Vendor (global) **Rule:** **max(Plant Criticality)** across all plants that MP Vendor supplies

It answers: _"How critical is this vendor to the enterprise overall?"_ If a vendor is HIGH at any single plant, its Vendor Criticality is HIGH — even if it's LOW at every other plant.

**Confirmed at 99.6%** across all three sheets combined. The 13 residual mismatches are all vendors rated HIGH with no HIGH plant criticality visible in the workbook — likely reflecting data from sites outside this extract or manual overrides.

---

### The Rollup in One Line

`Material Crit ──max per vendor+plant──▶ Plant Crit ──max per vendor──▶ Vendor Crit`