Raw Data - ZS163n from SAP No Filters
Geo-political Risk Score by Country -
	1) ICRG - International Country Risk Guide - Consolidated view from independent body that *corporate finance gets a report from* 
	2) USCIRF - Another measure of geopolitical risk taken from old strategy team (automatic source) -Religious Freedom [2026ARUSCIRF](https://www.uscirf.gov/sites/default/files/2026-03/USCIRF_2026_AR%20\(2\).pdf)
	- ZS163n referencing ICRG, analysis tab is taking maximum of the row items (worst SUPPLIER COUNTRY LOCATION)

### Spend Data - From Finance
#### Assumptions made: 
- Spend is measured as Invoiced Spend
- Posting date is taken to identify the time period
- Non-Pharmaceutical spend is excluded
- Formula to copy and paste the vendor name into any corresponding vendor parent that is blank
#### Applied filters: 
- Year is 2023
- Mapped Business Area Description is Pharmaceuticals
- Material Group Code is not MGC=BLANK 


### Material Group Code - From John Hamilton
- Paul to update about most up to date standards
- Commodity Code has Levels of specificity for all materials
	- Maybe gather from procurement the latest update
	- Clarify if the data is in SAP


Previous Year SoF 23 (REMOVE)
Spend (KEEP) - Corporate Finance
Material Group Criticality (KEEP) - Look in Ref-Unique (FROM JOHN HAMILTON)
- Columns W&X we did a lookup of what the category label was scored at 
Products (KEEP) - Same thing
- Launch products, volume, life saving, everything else
- Brooke has one for the conference
- Life Cycle Management owns the data 

Supplier Corporate Risk Location - Get rid of and replace with Everstream
- Everstream standard export
- Figure out how to specify what countries are riskier

Part Concentration - How concentrated is a supplier from one MP
-  Figure out logic (ME TO TAKE)


Vendor/Plant/Material Criticality