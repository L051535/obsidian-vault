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
		- Arturo to fix, the right way
3) Appended the unique products to the facility groups
4) Facility group Contract Mfg (D,P,P) needs changed because delimiter in Everstream parser uses commas
5) User Status Filter Changes: [[Everstream ZS163n Filters]]
6) Using plant_browser mfg & distribution, try to match to P codes to assign P codes with normalized names and categorize as "distirbution_center" vs "production_plant"
7) Removed P prefix while maintaining leading 0s
8) is_final_product column needs logic populated:
	- Instead of using procurement type, use plant_material combination of BOM master data table and apply following conditions:
		- If plant_material is in unique list of plant_component in master BOM table, is_final_product = "N"
		- Else "Y"
9) Filtered out all material from material data using MRP Type = "ND" && MRP Controller = "090"
10) Material flow classifications:
	- Check if a component is also a material in the same plant. This makes it an internal-intermediate
	- Check after P-Code stripping to see if MP Vendor codes 
11) Added DC data to facility gorups
12) SKU Browser vendor ID to fill ZS163n gaps
13) Used SAP Transaction ZS418 Table Query -> T001W to Geo-Locate internal sites