---
type: technical
date: 2026-04-01
status:
tags:
  - type/technical
  - initiative/everstream
  - SC-Resiliency-Sustainability
---

# Main Manual Changes Implemented
1) Using the RAW SAP Export of ZS163n instead of the SHARP database data
	- Reasoning is because SHARP adds additional filters removing a bunch
2) Added Meta_Products field into "Material" data table
	- Combined my approach of BOM explosions with Arturo's to cover the most amount of materials per product.
3) Appended the unique products to the facility groups
4) Facility group Contract Mfg (D,P,P) needs changed because delimiter in Everstream parser uses commas
5) User Status Filter Changes: [[Everstream ZS163n Filters]]
6) Using plant_browser mfg & distribution, try to match to P codes to assign P codes with normalized names and categorize as "distirbution_center" vs "production_plant"
7) Removed P Code on production_plant