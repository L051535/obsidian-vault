# Supplier Risk Assessment
## Replication Instructions — 0–10 Scoring Scale | Equal Weighting

---

## 1. Overview

This document provides step-by-step instructions to replicate the supplier-level risk assessment. The model scores 830 suppliers across six risk factors on a 0–10 scale and produces an equally-weighted composite score used to classify suppliers as HIGH, MEDIUM, or LOW risk.

---

## 2. Required Input Files

The following six files must be present in the project directory before running the analysis:

| File Name | Required Columns / Tabs | Key Field(s) | Notes |
|---|---|---|---|
| Spend_Data.xlsx | Vendor Code, Spend in USD | Vendor Code | One row per vendor; spend summed to supplier level |
| Main_Data_Table.xlsx | Supplier, MP Vendor, Material, Material Criticality, Product, MGC Description, Vendor Name1 | Supplier, MP Vendor, Material | Central join table — links all dimensions |
| Scoring_Reference.xlsx | Tab 1: Material Group Segmentation (Material Group, Score) / Tab 2: Product Segmentation (Product, Score) | Material Group; Product | Score values must be 1 (LOW), 2 (MEDIUM), 3 (HIGH) |
| Geopolitical_Score.xlsx | Country, Code, ICRG Score, Risk, Score. | Code (ISO 2-letter) | Score. column: 1=Low, 2=Medium, 3=High |
| ZS163n_Filtered_*.xlsx | Vendor, MP Vendor, Partner Function, Country/Region Key | Vendor, Partner Function = MP | Used only for supplier country lookup |
| MKVZ_*.csv | Country/Region Key, Supplier, Supplier Name, Terms of Payment | Supplier | Not used in current scoring model — available for future use |

> ⚠ File naming conventions for ZS163n and MKVZ include a date suffix (e.g. ZS163n_Filtered_3_9_26.xlsx). Update the filename reference in the script when refreshing data.

---

## 3. Data Preparation Steps

### 3.1 Main_Data_Table.xlsx — Central Join Table

This file is the backbone of the analysis. Before running, verify:

- `Supplier` column contains numeric vendor codes (no text or blanks in active rows)
- `MP Vendor` column contains numeric plant/vendor codes
- `Material` column contains unique SAP material numbers
- `Material Criticality` values are one of: HIGH, MEDIUM, LOW, EXTRNL, NONE (EXTRNL and NONE are excluded from Supplier Criticality scoring)
- `MGC Description` values match the Material Group column in Scoring_Reference where possible
- `Product` values match the Product column in Scoring_Reference where possible

### 3.2 Scoring_Reference.xlsx — Score Mappings

Two tabs are required:

- **Material Group Segmentation**: columns `Material Group` (text) and `Score` (1, 2, or 3)
- **Product Segmentation**: columns `Product` (text) and `Score` (1, 2, or 3)

Score values map as follows: 1 = LOW criticality, 2 = MEDIUM criticality, 3 = HIGH criticality. These are then converted to 0–10 scale scores (2 / 5 / 10) during analysis.

### 3.3 ZS163n — Country Lookup

Filter to rows where `Partner Function = 'MP'` only. The `Vendor` column in these rows represents the supplier ID. `Country/Region Key` must be a 2-letter ISO country code matching the `Code` column in Geopolitical_Score.xlsx.

---

## 4. Scoring Rules

Each of the six factors is scored on a 0–10 scale. Missing or unmatched data defaults to 0. The composite score is the simple average of all six factors.

| Factor                        | Scoring Rule (0–10)                                                                                              | Aggregation at Supplier Level                                                                                                   | Source                              |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| 1. Spend (USD)                | Q1 (≤$4.3K) = 2 \| Q2 ($4.3K–$23.3K) = 5 \| Q3 ($23.3K–$131.4K) = 7 \| Q4 (>$131.4K) = 10 \| Missing or zero = 0 | Sum all Vendor Code rows to Supplier; apply quartile thresholds                                                                 | Spend_Data.xlsx                     |
| 2. Material Group Criticality | LOW (Score=1) = 2 \| MEDIUM (Score=2) = 5 \| HIGH (Score=3) = 10 \| Unmatched MGC = 0                            | Join MGC Description → Scoring_Reference; take MAX score across all supplier materials                                          | Main_Data_Table + Scoring_Reference |
| 3. Product Criticality        | LOW (Score=1) = 2 \| MEDIUM (Score=2) = 5 \| HIGH (Score=3) = 10 \| Unmatched product = 0                        | Join Product → Scoring_Reference; take MAX score across all supplier products                                                   | Main_Data_Table + Scoring_Reference |
| 4. Geopolitical Score (ICRG)  | ICRG Low (Score.=1) = 2 \| ICRG Medium (Score.=2) = 5 \| ICRG High (Score.=3) = 10 \| Missing = 0                | Match Vendor (MP rows only) → Country/Region Key → Geo Score; take MAX if multi-country                                         | ZS163n + Geopolitical_Score         |
| 5. Part Concentration         | ≤10% = 1 \| 11–25% = 3 \| 26–50% = 6 \| 51–75% = 8 \| >75% = 10                                                  | Count unique Materials per (Supplier, MP Vendor); divide by total unique Materials per Supplier; use highest single MP Vendor % | Main_Data_Table                     |
| 6. Supplier Criticality       | ((% HIGH × 3) + (% MED × 2) + (% LOW × 1)) / 3 × 10 → raw range 3.33–10.00                                       | Count unique Materials per criticality level (HIGH/MED/LOW only — exclude EXTRNL/NONE); compute weighted share                  | Main_Data_Table                     |
| Composite Score               | Simple average of all 6 factor scores = (F1+F2+F3+F4+F5+F6) / 6                                                  | Equal weighting — each factor = 1/6                                                                                             | All sources                         |

> ⚠ Spend quartile thresholds ($4.3K / $23.3K / $131.4K) are derived from the current Spend_Data dataset. These will shift when the spend file is refreshed. Recalculate quartiles from the new data each time.

---

## 5. Risk Tier Classification

| Risk Tier | Composite Score Range | Interpretation |
|---|---|---|
| HIGH | ≥ 7.0 | Immediate attention required. Multiple high-severity risk factors present. |
| MEDIUM | 4.0 – 6.9 | Monitor actively. Moderate exposure; consider mitigation actions. |
| LOW | < 4.0 | Lower priority. Standard oversight procedures apply. |

---

## 6. Prompt to Replicate Analysis

Use the following prompt when starting a new conversation with Claude to replicate this analysis. Upload all six input files to the project before submitting.

```
Build a supplier-level risk assessment using the six input files in this project. Score each supplier across these six factors on a 0-10 scale with equal weighting:

(1) Spend in USD from Spend_Data.xlsx using quartile thresholds [Q1=2, Q2=5, Q3=7, Q4=10, missing=0];

(2) Material Group Criticality from Main_Data_Table MGC Description joined to Scoring_Reference [LOW=2, MEDIUM=5, HIGH=10, unmatched=0], take MAX per supplier;

(3) Product Criticality from Main_Data_Table Product joined to Scoring_Reference [LOW=2, MEDIUM=5, HIGH=10, unmatched=0], take MAX per supplier;

(4) Geopolitical Score from ZS163n (MP rows only) joined to Geopolitical_Score.xlsx [ICRG Low=2, Medium=5, High=10, missing=0], take MAX if multi-country;

(5) Part Concentration using highest % of unique materials at a single MP Vendor [<=10%=1, 11-25%=3, 26-50%=6, 51-75%=8, >75%=10];

(6) Supplier Criticality using ((pct_HIGH x 3)+(pct_MED x 2)+(pct_LOW x 1))/3 x 10 on unique materials (HIGH/MEDIUM/LOW only, exclude EXTRNL/NONE).

Composite score = average of all 6 factors. Risk tiers: HIGH >= 7.0, MEDIUM 4.0-6.9, LOW < 4.0.

Output as a formatted Excel file with three tabs: Risk Assessment (sorted by composite score descending), Methodology, and Summary Stats.
```

---

## 7. Output File Structure

The output is a single Excel file (`Supplier_Risk_Assessment_v2.xlsx`) with three tabs:

| Tab | Contents |
|---|---|
| Risk Assessment | Full supplier list sorted by Composite Score (desc). Columns: Supplier ID, Name, Spend, all 6 factor scores, Composite Score, Risk Tier. Score cells shaded red/orange/green by value. |
| Methodology | Scoring rules, aggregation logic, and data sources for each factor. |
| Summary Stats | Tier counts, average composite scores, total/average spend per tier, and average factor scores broken down by risk tier. |

---

## 8. Known Data Issues & Decisions

| Item | Issue | Current Handling |
|---|---|---|
| MGC Description gaps | ~30 MGC values in Main_Data_Table do not match Scoring_Reference (e.g. Vials, Blisters, Drug substance ingredients) | Defaulted to score = 0 |
| Part Concentration skew | Majority of suppliers score 10 (>75% of parts at single MP Vendor) | By design — reflects actual single-source exposure |
| Supplier Criticality floor | Formula minimum is 3.33 (100% LOW materials), not 0 | Left as-is per design decision |
| Missing spend data | Not all suppliers in Main_Data_Table appear in Spend_Data | Missing spend defaults to score = 0 |

---

## 9. Refreshing the Analysis

When source data is updated, follow these steps:

1. Replace the relevant input file(s) in the project directory with the new version.
2. If refreshing Spend_Data.xlsx, note that spend quartile thresholds will recalculate automatically from the new data.
3. If refreshing ZS163n, update the filename reference in the prompt to match the new date suffix.
4. If adding new MGC Description values, check whether they map to an existing Material Group in Scoring_Reference. If not, add the mapping before running.
5. Submit the prompt from Section 6 in a new Claude conversation with all updated files attached.

> ⚠ Risk tier thresholds (7.0 / 4.0) and factor weights are fixed in the current model. If you wish to adjust them, specify the new values explicitly in the prompt.
