---
date: 2026-05-07
status:
type:
tags:
---
1) ZS163n Data Pull all unique Vendor IDs after following filters:
	- User Status in: [[Everstream ZS163n Filters]]
	- Sub-Filter on User Status SAPR for Material Code/Description in:
		GMPSERVICE030 - Transportation
		GMPSERVICE052 - Transportation, Logistics, &Distribution
		GMPSERVICE025 - Storage
		GMPSERVICE011 - Distribution
		GMPSERVICE055 - DISTRIBUTION-SPECIAL SECURITY SUBSTANCES
		GMPSERVICE007 - Construction
		GMPSERVICE074 - HIGH SECURITY TRANSPORTATION
		GMPSERVICE064 - ACTIVE SUBSTANCES TRANSPORT
		GMPSERVICE056 - DISTRIBUTION-CONTROLLED DRUGS
		GMPSERVICE068 - STORAGE - SPECIAL SECURITY SUBSTANCES
		GMPSERVICE066 - RADIOACTIVE MATERIAL TRANSPORT
		GMPSERVICE069 - STORAGE - CONTROLLED SUBSTANCES
		
2) Pull all Unique Plant IDs
3) Remove all facilities with ELXA in name (they are financial flow sites for transfer pricing)
4) Use ZS418 -> T001W SAP report to fill in remaining missing addresses if not in ZS163n
5) Use Arturo's Material Code & My Product Association to populate groups
6) Populate warehouses with "Warehouse" group tag
7) populate contract manufacturers (AMFG User Status) with "Contract Manufacturing" group tag
8) Manually add warehouses & DCs after confirming with logistics & transportation and properly tag in type
9) Manually add TPRM facilities and label with "TPRM Monitoring" group tag
10) Ensure addresses, city, country not blank otherwise include "." to fill and add longitude & latitude estimates