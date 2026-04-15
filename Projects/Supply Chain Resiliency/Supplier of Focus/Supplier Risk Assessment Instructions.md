---
date: 2026-04-15
status:
type:
tags:
---
# Supplier Risk Assessment
## Replication Instructions — 0–10 Scoring Scale | Equal Weighting | v4

---
## 1. Overview

This document provides step-by-step instructions to replicate the supplier-level risk assessment. The model scores **830 suppliers** across **six risk factors** on a 0–10 scale and produces an equally-weighted composite score used to classify suppliers as HIGH, MEDIUM, or LOW risk.

**Key change from prior version:** Geopolitical Score (ICRG) has been replaced by **External Risk Score** sourced from the Facilities Views Report. The facility-level External Risk score is matched to each supplier's MP Vendor(s) and provides more granular, facility-specific risk data than the prior country-level ICRG score.

---

## 2. Required Input Files

| File Name | Required Columns / Tabs | Key Field(s) | Notes |
|---|---|---|---|
| Spend_Data.xlsx | Vendor Code, Spend in USD | Vendor Code | One row per vendor; spend summed to supplier level |
| Main_Data_Table.xlsx | Supplier, MP Vendor, Material, Material Criticality, Product, MGC Description, Vendor Name1 | Supplier, MP Vendor, Material | Central join table — links all dimensions |
| Scoring_Reference.xlsx | Tab 1: Material Group Segmentation (Material Group, Score) / Tab 2: Product Segmentation (Product, Score) | Material Group; Product | Score values must be 1 (LOW), 2 (MEDIUM), 3 (HIGH) |
| Facilities_Views_Report_*.xlsx | Facility Name, External Risks | Facility Name | Supplier ID is embedded at end of Facility Name (e.g. "SUPPLIER NAME 1000571101"). External Risks range: 4–13. |
| ZS163n_Filtered_*.xlsx | Vendor, MP Vendor, Partner Function, Country/Region Key | Vendor, Partner Function = MP | No longer used for scoring — retained for reference |
| MKVZ_*.csv | Country/Region Key, Supplier, Supplier Name, Terms of Payment | Supplier | Not used in current scoring model — available for future use |

> ⚠ The Facilities Views Report filename includes a timestamp suffix (e.g. `Facilities_Views_Report_20260415165944.xlsx`). Update the filename reference when refreshing data.

> ⚠ ZS163n is no longer required for scoring but retained in the project for reference. If removed, no scoring logic is affected.

---

## 3. Data Preparation Steps

### 3.1 Main_Data_Table.xlsx — Central Join Table

This file is the backbone of the analysis. Before running, verify:

- `Supplier` column contains numeric vendor codes (no text or blanks in active rows)
- `MP Vendor` column contains numeric plant/vendor codes
- `Material` column contains unique SAP material numbers
- `Material Criticality` values are one of: HIGH, MEDIUM, LOW, EXTRNL, NONE (EXTRNL and NONE are excluded from Supplier Criticality scoring)
- `MGC Description` values match the `Material Group` column in Scoring_Reference where possible
- `Product` values match the `Product` column in Scoring_Reference where possible

### 3.2 Scoring_Reference.xlsx — Score Mappings

Two tabs are required:

- **Material Group Segmentation**: columns `Material Group` (text) and `Score` (1, 2, or 3)
- **Product Segmentation**: columns `Product` (text) and `Score` (1, 2, or 3)

Score values map as follows: 1 = LOW, 2 = MEDIUM, 3 = HIGH. These are converted to 0–10 scale scores (2 / 5 / 10) during analysis.

### 3.3 Facilities_Views_Report — Facility ID Extraction

The `Facility Name` column contains the supplier/vendor ID appended at the end of the name as a numeric string:

```
"MERCK SERONO SA 1000571101"       → Extracted ID: 1000571101
"CORNING LIFE SCIENCES 1000571105" → Extracted ID: 1000571105
"BRUKER OPTICS INC 100004"         → Extracted ID: 100004
```

**Extraction rule:** Use a regex pattern to find a 6–10 digit number at the end of the string: `r'(\d{6,10})\s*$'`

Internal Lilly facilities (e.g. `[KINSALE]`, `[IDAP]`) have no numeric ID and are excluded from matching — they are bracketed names with no supplier association.

**Current coverage:**
- 1,731 of 1,923 facilities have an extractable ID
- 1,463 of 3,444 unique MP Vendors in Main_Data_Table are matched to a facility
- 688 of 830 suppliers have at least one MP Vendor matched
- 141 suppliers default to External Risk Score = 0

---

## 4. Scoring Rules

Each of the six factors is scored on a 0–10 scale. **Missing or unmatched data defaults to 0.** The composite score is the simple average of all six factors.

| Factor | Scoring Rule (0–10) | Aggregation at Supplier Level | Source |
|---|---|---|---|
| 1. Spend (USD) | Q1 (≤$4.3K) = 2 \| Q2 ($4.3K–$23.3K) = 5 \| Q3 ($23.3K–$131.4K) = 7 \| Q4 (>$131.4K) = 10 \| Missing or zero = 0 | Sum all Vendor Code rows to Supplier; apply quartile thresholds | Spend_Data.xlsx |
| 2. Material Group Criticality | LOW (Score=1) = 2 \| MEDIUM (Score=2) = 5 \| HIGH (Score=3) = 10 \| Unmatched = 0 | Join MGC Description → Scoring_Reference; take MAX across all supplier materials | Main_Data_Table + Scoring_Reference |
| 3. Product Criticality | LOW (Score=1) = 2 \| MEDIUM (Score=2) = 5 \| HIGH (Score=3) = 10 \| Unmatched = 0 | Join Product → Scoring_Reference; take MAX across all supplier products | Main_Data_Table + Scoring_Reference |
| 4. External Risk Score | Raw score 4–13 normalized linearly to 0–10. Formula: `(raw − 4) / 9 × 10`. Missing = 0 | Extract ID from Facility Name; match to MP Vendor; take MAX across all matched MP Vendors per supplier | Facilities_Views_Report.xlsx |
| 5. Part Concentration | ≤10% = 1 \| 11–25% = 3 \| 26–50% = 6 \| 51–75% = 8 \| >75% = 10 | Count unique Materials per (Supplier, MP Vendor); divide by total unique Materials per Supplier; use highest single MP Vendor % | Main_Data_Table |
| 6. Supplier Criticality | `((% HIGH × 3) + (% MED × 2) + (% LOW × 1)) / 3 × 10` → raw range 3.33–10.00 | Count unique Materials per criticality level (HIGH/MED/LOW only — exclude EXTRNL/NONE); compute weighted share | Main_Data_Table |
| Composite Score | Simple average of all 6 factor scores = (F1+F2+F3+F4+F5+F6) / 6 | Equal weighting — each factor = 1/6 | All sources |

> ⚠ Spend quartile thresholds ($4.3K / $23.3K / $131.4K) are derived from the current Spend_Data dataset. Recalculate from new data each time the spend file is refreshed.

### External Risk Normalization Reference

| Raw External Risk Score | Normalized 0–10 Score |
|---|---|
| 4 | 0.00 |
| 5 | 1.11 |
| 6 | 2.22 |
| 7 | 3.33 |
| 8 | 4.44 |
| 9 | 5.56 |
| 10 | 6.67 |
| 11 | 7.78 |
| 12 | 8.89 |
| 13 | 10.00 |

---

## 5. Risk Tier Classification

| Risk Tier | Composite Score Range | Interpretation |
|---|---|---|
| HIGH | ≥ 7.0 | Immediate attention required. Multiple high-severity risk factors present. |
| MEDIUM | 4.0 – 6.9 | Monitor actively. Moderate exposure; consider mitigation actions. |
| LOW | < 4.0 | Lower priority. Standard oversight procedures apply. |

---

## 6. Product & Material Group Indicator Columns

In addition to the six scored factors, the output includes **Y/blank indicator columns** showing which products and material groups each supplier is associated with.

**Products — HIGH (Score 3):** DONANEMAB, EBGLYSS, JAYPIRCA, LEBRIKIZUMAB, MOUNJARO, OMVOH, SERD, ZEPBOUND

**Products — MEDIUM (Score 2):** BASAGLAR, CYRAMZA, HUMALOG, HUMULIN, OLUMIANT, TALTZ, TRULICITY, VERZENIO

**Products — OTHER:** Single column; Y if the supplier has any LOW-scored product material

**Material Groups — HIGH (Score 3):** Active Ingredients, Excipients, Glass Packaging, Medical Devices, MRO, Semi-Fin Cont Manuf, Specialty Chemicals

**Material Groups — MEDIUM (Score 2):** Blister Films, Blister Foils, Capsules & Pulvules, Drums and Pallets, Lab Consumables, Plastic Bottles, Rubber seal/stoppers, Screw Caps

**Material Groups — OTHER:** Single column; Y if the supplier has any LOW-scored material group

A `Y` means the supplier has **at least one material** associated with that product or material group in Main_Data_Table.

---

## 7. Prompt to Replicate Analysis

Use the following prompt when starting a new conversation with Claude. Add all required input files to the project before submitting.

```
Build a supplier-level risk assessment using the input files in this project.
Score each supplier across six factors on a 0-10 scale with equal weighting:

(1) Spend in USD from Spend_Data.xlsx using quartile thresholds
    [Q1=2, Q2=5, Q3=7, Q4=10, missing/zero=0]

(2) Material Group Criticality from Main_Data_Table MGC Description
    joined to Scoring_Reference Material Group Segmentation tab
    [LOW=2, MEDIUM=5, HIGH=10, unmatched=0], take MAX per supplier

(3) Product Criticality from Main_Data_Table Product
    joined to Scoring_Reference Product Segmentation tab
    [LOW=2, MEDIUM=5, HIGH=10, unmatched=0], take MAX per supplier

(4) External Risk Score from Facilities_Views_Report.xlsx:
    - Extract the numeric vendor ID from the end of each Facility Name
      using regex pattern r'(\d{6,10})\s*$'
    - Match extracted ID to MP Vendor in Main_Data_Table
    - Normalize raw External Risks score (range 4-13) to 0-10
      using formula: (raw - 4) / 9 * 10
    - Take MAX normalized score across all matched MP Vendors per supplier
    - Missing/unmatched = 0

(5) Part Concentration using highest % of unique materials at a single
    MP Vendor [<=10%=1, 11-25%=3, 26-50%=6, 51-75%=8, >75%=10]

(6) Supplier Criticality using
    ((pct_HIGH x 3) + (pct_MED x 2) + (pct_LOW x 1)) / 3 x 10
    on unique materials per supplier (HIGH/MEDIUM/LOW only,
    exclude EXTRNL and NONE). Natural range is 3.33-10; leave as-is.

Composite score = simple average of all 6 factors (0-10 scale).
Risk tiers: HIGH >= 7.0, MEDIUM 4.0-6.9, LOW < 4.0.
All missing/unmatched data defaults to 0.

Also add Y/blank indicator columns for each HIGH and MEDIUM product
and material group (from Scoring_Reference). Group all LOW-scored
products into one "Other" column and all LOW-scored material groups
into one "Other" column. A Y means the supplier has at least one
material for that product/material group in Main_Data_Table.

Output a formatted Excel file with three tabs:
- Risk Assessment: sorted by composite score descending, with score
  cells shaded red/orange/green and indicator columns grouped and
  color-coded by tier
- Methodology: scoring rules, aggregation logic, data sources
- Summary Stats: tier counts, average scores, spend by tier
```

---

## 8. Output File Structure

| Tab | Contents |
|---|---|
| Risk Assessment | Full supplier list sorted by Composite Score (desc). Columns: Supplier ID, Name, Spend, Ext. Risk Raw, all 6 factor scores, Composite Score, Risk Tier, then Y/blank indicators for all HIGH/MEDIUM products and MGC groups. Score cells shaded red/orange/green. |
| Methodology | Scoring rules, aggregation logic, normalization formulas, and data sources for each factor. Includes facility match coverage note. |
| Summary Stats | Tier counts, % of total, average composite scores, average and total spend per tier. Average factor score breakdown by risk tier. |

---

## 9. Known Data Issues & Decisions

| Item | Issue | Current Handling |
|---|---|---|
| MGC Description gaps | ~30 MGC values in Main_Data_Table do not match Scoring_Reference (e.g. Vials, Blisters, Drug substance ingredients, Contract Manufacturing entries) | Defaulted to score = 0 |
| Facility match coverage | 1,463 of 3,444 MP Vendors matched; 141 suppliers have no facility match | Missing External Risk defaults to 0 |
| Part Concentration skew | Majority of suppliers score 10 (>75% of parts at single MP Vendor) | By design — reflects actual single-source exposure |
| Supplier Criticality floor | Formula minimum is 3.33 (100% LOW materials), not 0 | Left as-is per design decision |
| Missing spend data | Not all suppliers in Main_Data_Table appear in Spend_Data | Missing spend defaults to score = 0 |
| Internal Lilly facilities | Bracketed names (e.g. [KINSALE], [IDAP]) have no numeric ID and are excluded from matching | Expected — these are internal sites not external suppliers |

---

## 10. Refreshing the Analysis

When source data is updated, follow these steps:

1. Replace the relevant input file(s) in the project directory with the new version.
2. If refreshing **Spend_Data.xlsx**, spend quartile thresholds will recalculate automatically from the new data.
3. If refreshing the **Facilities Views Report**, update the filename in the prompt to match the new timestamp suffix.
4. If refreshing **ZS163n**, no scoring logic is affected (file no longer used in scoring).
5. If adding new **MGC Description** values, check whether they map to an existing Material Group in Scoring_Reference. If not, add the mapping before running.
6. If the **External Risks score range** changes beyond 4–13 in a new data pull, update the normalization formula min/max accordingly.
7. Submit the prompt from Section 7 in a new Claude conversation with all updated files attached.

> ⚠ Risk tier thresholds (7.0 / 4.0), factor weights, and scoring anchors are fixed in the current model. Specify new values explicitly in the prompt if you wish to adjust them.

---

## 11. Version History

| Version | Change |
|---|---|
| v1 | Initial build. 1–3 scale, equal weighting. |
| v2 | Upgraded to 0–10 scale. Spend on quartiles (2/5/7/10). MGC/Product/Geo mapped to 2/5/10. Part Concentration to 5-bucket scale. |
| v3 | Added Y/blank indicator columns for HIGH/MEDIUM products and material groups. LOW grouped as "Other". |
| v4 | Replaced Geopolitical Score (ICRG country-level) with External Risk Score from Facilities Views Report (facility-level). Matched via numeric ID extracted from Facility Name to MP Vendor. Normalized raw score 4–13 to 0–10. |
