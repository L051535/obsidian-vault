---
date: 2026-04-15
status:
type:
tags:
---
# Supplier Risk Assessment
## Replication Instructions — 0–10 Scoring Scale | Custom Weighting | v5

---

## 1. Overview

This document provides step-by-step instructions to replicate the supplier-level risk assessment. The model scores **830 suppliers** across **six risk factors** on a 0–10 scale and produces a **weighted composite score** used to classify suppliers as HIGH, MEDIUM, or LOW risk.

**Key changes from v4:**
- Weights are now **fully customizable** — stored on a dedicated Weights tab and referenced by live Excel formulas
- The Composite Score column is a **live formula** ==(=D*weight + E*weight + ...)== that recalculates instantly when weights are edited
- The Risk Tier column is a **live formula** ==(=IF(composite>=7,"HIGH",...))== that updates with the composite score
- Default weights used in v5: Spend 15% | Material Group 10% | Product 25% | External Risk 20% | Part Concentration 15% | Supplier Criticality 15%

---

## 2. Required Input Files

| File Name | Required Columns / Tabs | Key Field(s) | Notes |
|---|---|---|---|
| Spend_Data.xlsx | Vendor Code, Spend in USD | Vendor Code | One row per vendor; spend summed to supplier level |
| Main_Data_Table.xlsx | Supplier, MP Vendor, Material, Material Criticality, Product, MGC Description, Vendor Name1 | Supplier, MP Vendor, Material | Central join table — links all dimensions |
| Scoring_Reference.xlsx | Tab 1: Material Group Segmentation (Material Group, Score) / Tab 2: Product Segmentation (Product, Score) | Material Group; Product | Score values: 1=LOW, 2=MEDIUM, 3=HIGH |
| Facilities_Views_Report_*.xlsx | Facility Name, External Risks | Facility Name | Vendor ID embedded at end of Facility Name. External Risks range: 4–13. |
| ZS163n_Filtered_*.xlsx | Vendor, MP Vendor, Partner Function, Country/Region Key | Reference only | No longer used in scoring. Retained for reference. |
| MKVZ_*.csv | Country/Region Key, Supplier, Supplier Name, Terms of Payment | Supplier | Not used in current scoring model. |

> ⚠ The Facilities Views Report filename includes a timestamp suffix (e.g. `Facilities_Views_Report_20260415165944.xlsx`). Update the filename reference when refreshing data.

---

## 3. Data Preparation Steps

### 3.1 Main_Data_Table.xlsx — Central Join Table

Before running, verify:

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

Score values: 1 = LOW, 2 = MEDIUM, 3 = HIGH. Converted to 0–10 scale scores (2 / 5 / 10) during analysis.

### 3.3 Facilities_Views_Report — Facility ID Extraction

The `Facility Name` column contains the supplier/vendor ID appended as a numeric string at the end:

```
"MERCK SERONO SA 1000571101"       → Extracted ID: 1000571101
"CORNING LIFE SCIENCES 1000571105" → Extracted ID: 1000571105
"BRUKER OPTICS INC 100004"         → Extracted ID: 100004
```

**Extraction rule:** Regex pattern matching a 6–10 digit number at the end of the string: `r'(\d{6,10})\s*$'`

Internal Lilly facilities (e.g. `[KINSALE]`, `[IDAP]`) use bracketed names with no numeric ID and are automatically excluded.

**Current coverage:** 1,731 of 1,923 facilities have an extractable ID. 1,463 of 3,444 MP Vendors matched. 688 of 830 suppliers have at least one match. 141 suppliers default to score = 0.

---

## 4. Scoring Rules

Each factor is scored on a 0–10 scale. **Missing or unmatched data defaults to 0.**

| Factor                        | Scoring Rule (0–10)                                                                                           | Aggregation at Supplier Level                                                                          | Source                              |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ----------------------------------- |
| 1. Spend (USD)                | Q1 (≤$4.3K) = 2 \| Q2 ($4.3K–$23.3K) = 5 \| Q3 ($23.3K–$131.4K) = 7 \| Q4 (>$131.4K) = 10 \| Missing/zero = 0 | Sum all Vendor Code rows to Supplier; apply quartile thresholds                                        | Spend_Data.xlsx                     |
| 2. Material Group Criticality | LOW (Score=1) = 2 \| MEDIUM (Score=2) = 5 \| HIGH (Score=3) = 10 \| Unmatched = 0                             | Join MGC Description → Scoring_Reference; take MAX across all supplier materials                       | Main_Data_Table + Scoring_Reference |
| 3. Product Criticality        | LOW (Score=1) = 2 \| MEDIUM (Score=2) = 5 \| HIGH (Score=3) = 10 \| Unmatched = 0                             | Join Product → Scoring_Reference; take MAX across all supplier products                                | Main_Data_Table + Scoring_Reference |
| 4. External Risk Score        | Raw score 4–13 normalized to 0–10. Formula: `(raw − 4) / 9 × 10`. Missing = 0                                 | Extract ID from Facility Name; match to MP Vendor; take MAX across all matched MP Vendors per supplier | Facilities_Views_Report.xlsx        |
| 5. Part Concentration         | ≤10% = 1 \| 11–25% = 3 \| 26–50% = 6 \| 51–75% = 8 \| >75% = 10                                               | Count unique Materials per (Supplier, MP Vendor); divide by total; use highest single MP Vendor %      | Main_Data_Table                     |
| 6. Supplier Criticality       | `((% HIGH × 3) + (% MED × 2) + (% LOW × 1)) / 3 × 10` → range 3.33–10.00                                      | Count unique Materials per criticality level (HIGH/MED/LOW only); compute weighted share               | Main_Data_Table                     |

> ⚠ Spend quartile thresholds ($4.3K / $23.3K / $131.4K) are derived from the current Spend_Data dataset. Recalculate from new data each time the spend file is refreshed.

### External Risk Normalization Reference

| Raw Score | Normalized (0–10) | Raw Score | Normalized (0–10) |
|---|---|---|---|
| 4 | 0.00 | 9 | 5.56 |
| 5 | 1.11 | 10 | 6.67 |
| 6 | 2.22 | 11 | 7.78 |
| 7 | 3.33 | 12 | 8.89 |
| 8 | 4.44 | 13 | 10.00 |

---

## 5. Composite Score — Weighted Formula

The composite score is a **weighted sum** of the six factor scores, not a simple average.

```
Composite = (Spend × W1) + (MGC × W2) + (Product × W3)
          + (Ext Risk × W4) + (Concentration × W5) + (Supplier Criticality × W6)
```

**Weights must sum to 1.0 (100%).**

### Default weights (v5)

| Factor | Weight | Column |
|---|---|---|
| Spend | 15% (0.15) | D — Weights!$B$4 |
| Material Group Criticality | 10% (0.10) | E — Weights!$B$5 |
| Product Criticality | 25% (0.25) | F — Weights!$B$6 |
| External Risk | 20% (0.20) | H — Weights!$B$7 |
| Part Concentration | 15% (0.15) | J — Weights!$B$8 |
| Supplier Criticality | 15% (0.15) | N — Weights!$B$9 |

### Excel formula (per row)

```
=D{row}*Weights!$B$4 + E{row}*Weights!$B$5 + F{row}*Weights!$B$6
 + H{row}*Weights!$B$7 + J{row}*Weights!$B$8 + N{row}*Weights!$B$9
```

### Risk Tier formula (per row)

```
=IF(O{row}>=7,"HIGH",IF(O{row}>=4,"MEDIUM","LOW"))
```

---

## 6. Risk Tier Classification

| Risk Tier | Composite Score Range | Interpretation |
|---|---|---|
| HIGH | ≥ 7.0 | Immediate attention required. Multiple high-severity risk factors present. |
| MEDIUM | 4.0 – 6.9 | Monitor actively. Moderate exposure; consider mitigation actions. |
| LOW | < 4.0 | Lower priority. Standard oversight procedures apply. |

---

## 7. How to Change Weights in the Excel Output

1. Open the output Excel file and navigate to the **Weights** tab
2. The six weight values are in **column B, rows 4–9** (highlighted yellow with blue text)
3. Edit any weight value — e.g. change `0.15` to `0.20`
4. Ensure the total in **B10** still shows `1.00` — if not, adjust other weights accordingly
5. The **Composite Score** column on the Risk Assessment tab recalculates instantly
6. The **Risk Tier** column updates automatically as composite scores change

> ⚠ The Risk Tier cell background color (red/orange/green) does not auto-update — only the text label updates dynamically. Sort or filter by the Risk Tier column after changing weights to re-group suppliers correctly.

---

## 8. Product & Material Group Indicator Columns

Y/blank indicator columns show which products and material groups each supplier is associated with. A `Y` means the supplier has at least one material for that product or material group in Main_Data_Table.

**Products — HIGH (Score 3):** DONANEMAB, EBGLYSS, JAYPIRCA, LEBRIKIZUMAB, MOUNJARO, OMVOH, SERD, ZEPBOUND

**Products — MEDIUM (Score 2):** BASAGLAR, CYRAMZA, HUMALOG, HUMULIN, OLUMIANT, TALTZ, TRULICITY, VERZENIO

**Products — OTHER:** Single column; Y if the supplier has any LOW-scored product material

**Material Groups — HIGH (Score 3):** Active Ingredients, Excipients, Glass Packaging, Medical Devices, MRO, Semi-Fin Cont Manuf, Specialty Chemicals

**Material Groups — MEDIUM (Score 2):** Blister Films, Blister Foils, Capsules & Pulvules, Drums and Pallets, Lab Consumables, Plastic Bottles, Rubber seal/stoppers, Screw Caps

**Material Groups — OTHER:** Single column; Y if the supplier has any LOW-scored material group

---

## 9. Prompt to Replicate Analysis

Use the following prompt when starting a new conversation with Claude. Add all required input files to the project before submitting. Edit the weights section to match your desired weighting.

```
Build a supplier-level risk assessment using the input files in this project.
Score each supplier across six factors on a 0-10 scale with custom weighting:

Weights (must sum to 1.0):
  Spend:                0.15
  Material Group:       0.10
  Product Criticality:  0.25
  External Risk:        0.20
  Part Concentration:   0.15
  Supplier Criticality: 0.15

Scoring rules for each factor:
(1) Spend: Q1(<=4.3K)=2, Q2(4.3K-23.3K)=5, Q3(23.3K-131.4K)=7, Q4(>131.4K)=10, missing=0
(2) Material Group Criticality: LOW=2, MEDIUM=5, HIGH=10, unmatched=0, take MAX per supplier
(3) Product Criticality: LOW=2, MEDIUM=5, HIGH=10, unmatched=0, take MAX per supplier
(4) External Risk from Facilities_Views_Report: extract numeric ID from end of Facility Name
    using regex r'(\d{6,10})\s*$', match to MP Vendor in Main_Data_Table, normalize raw
    External Risks score (range 4-13) to 0-10 via (raw-4)/9*10, take MAX per supplier, missing=0
(5) Part Concentration: <=10%=1, 11-25%=3, 26-50%=6, 51-75%=8, >75%=10
(6) Supplier Criticality: ((pct_HIGH*3)+(pct_MED*2)+(pct_LOW*1))/3*10,
    HIGH/MEDIUM/LOW materials only (exclude EXTRNL and NONE), range 3.33-10, leave as-is

Composite score = weighted sum of all 6 factors (not a simple average).
Risk tiers: HIGH >= 7.0, MEDIUM 4.0-6.9, LOW < 4.0. All missing data defaults to 0.

Also add Y/blank indicator columns for each HIGH and MEDIUM product and material group
(from Scoring_Reference). Group all LOW-scored products into one "Other" column and all
LOW-scored material groups into one "Other" column.

Output a formatted Excel file with four tabs:
- Weights: editable weight table (B4:B9) with yellow highlighted cells and a SUM total
  in B10. Weights reference format: Spend=B4, MGC=B5, Product=B6, ExtRisk=B7, Conc=B8, SC=B9
- Risk Assessment: sorted by composite score descending. Composite Score column must be a
  live Excel formula referencing the Weights sheet (e.g. =D5*Weights!$B$4+E5*Weights!$B$5+...).
  Risk Tier column must also be a live formula (=IF(O5>=7,"HIGH",IF(O5>=4,"MEDIUM","LOW"))).
  Score cells shaded red/orange/green. Indicator columns grouped and color-coded by tier.
- Methodology: scoring rules, weight references, and formula documentation
- Summary Stats: tier counts, average scores, spend by tier
```

---

## 10. Output File Structure

| Tab | Contents |
|---|---|
| Weights | Six editable weight cells (B4:B9, yellow/blue), SUM total in B10, As % column. Edit here to instantly recalculate composite scores. |
| Risk Assessment | Full supplier list sorted by Composite Score (desc). Score columns D/E/F/H/J/N are hardcoded values. Column O (Composite) is a live formula. Column P (Risk Tier) is a live formula. Indicator Y/blank columns follow. |
| Methodology | Scoring rules, weight references (Weights!B4:B9), formula structure, and data source for each factor. |
| Summary Stats | Tier counts, % of total, average composite scores, average and total spend per tier. Average factor scores by tier. Note: tier counts are static at export time — re-sort after weight changes. |

---

## 11. Known Data Issues & Decisions

| Item | Issue | Current Handling |
|---|---|---|
| MGC Description gaps | ~30 MGC values do not match Scoring_Reference (e.g. Vials, Blisters, Contract Mfg entries) | Defaulted to score = 0 |
| Facility match coverage | 1,463 of 3,444 MP Vendors matched; 141 suppliers have no facility match | Missing External Risk defaults to 0 |
| Part Concentration skew | Majority of suppliers score 10 (>75% of parts at single MP Vendor) | By design — reflects actual single-source exposure |
| Supplier Criticality floor | Formula minimum is 3.33 (100% LOW materials), not 0 | Left as-is per design decision |
| Missing spend data | Not all suppliers in Main_Data_Table appear in Spend_Data | Missing spend defaults to 0 |
| Internal Lilly facilities | Bracketed names (e.g. [KINSALE]) have no numeric ID | Excluded from matching — expected behaviour |
| Risk Tier cell color | Background color does not auto-update when weights change | Text label updates; re-sort by Risk Tier column after weight changes |
| Summary Stats tier counts | Static values captured at export time | Re-run analysis or manually update after significant weight changes |

---

## 12. Refreshing the Analysis

1. Replace the relevant input file(s) in the project directory with the new version.
2. If refreshing **Spend_Data.xlsx**, spend quartile thresholds will recalculate automatically.
3. If refreshing the **Facilities Views Report**, update the filename in the prompt to match the new timestamp suffix.
4. If adding new **MGC Description** values, check whether they map to an existing Material Group in Scoring_Reference before running.
5. If the **External Risks score range** changes beyond 4–13, update the normalization formula min/max accordingly.
6. Submit the prompt from Section 9 in a new Claude conversation with all updated files attached.
7. After receiving the new Excel file, go to the **Weights tab** and enter your desired weights before sharing.

> ⚠ Risk tier thresholds (7.0 / 4.0) are fixed in the formula. To change them, specify new values explicitly in the prompt and the IF formula will be updated accordingly.

---

## 13. Version History

| Version | Change |
|---|---|
| v1 | Initial build. 1–3 scale, equal weighting. |
| v2 | Upgraded to 0–10 scale. Spend on quartiles (2/5/7/10). MGC/Product/Geo mapped to 2/5/10. Part Concentration to 5-bucket scale. |
| v3 | Added Y/blank indicator columns for HIGH/MEDIUM products and material groups. LOW grouped as "Other". |
| v4 | Replaced Geopolitical Score (ICRG country-level) with External Risk Score from Facilities Views Report. Matched via numeric ID extracted from Facility Name to MP Vendor. Normalized raw score 4–13 to 0–10. |
| v5 | Custom weighting. Added dedicated Weights tab with editable cells (B4:B9). Composite Score column is now a live Excel formula referencing Weights sheet. Risk Tier column is a live IF formula. Default weights: Spend 15%, MGC 10%, Product 25%, Ext Risk 20%, Conc 15%, SC 15%. |
