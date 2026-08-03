---
date: 2026-08-03
status:
type:
tags:
---
# Transactions:

1. VL06O — Outbound Deliveries — DOMESTIC EACH. 
	- DELIVERY TYPE = ZL01
	- Change "Data" to "Sheet1"
	- Change Actual delivery qty to Delivery Quantity
2. VL06O — Outbound Deliveries — EXPORT EACHES 
	- DELIVERY TYPE = ZL02 to ZL03
3. VL06O — Outbound Deliveries — NALO SAMPLES. 
	- DELIVERY TYPE = ZL11
4. Vl06O — Outbound Deliveries — PDC TO 3PL. 
	- WAREHOUSE NUMBER = 141, SHIP TO PARTY = P04200003
5. ~={red}ZV422 — Variant “USPROMOSHIP.”=~
	- Discontinued and no longer in warehouse (promo materials)

# Steps
1. Access S4P and enter in VL06O
2. Select list outbound deliveries