---
doc_number: PRD-86780
version: "17.0"
status: Effective
effective_date: 2025-01-18
training_id: USD-SOP-Mtl Mvt btw 151 and PDC-001
tags:
  - nalo/sop
  - nalo/distribution
---

# Material Movement between Building 151 and the Plainfield Distribution Center

**Supersedes:** 001-005187
**Areas Involved:** North American Logistics Operations (NALO) - Plainfield, NALO Quality Assurance, and Delivery Services
**Areas Affected:** Indy Device Assembly and Packaging

## Purpose

To establish the process for transferring approved finished and semi-finished product between Building 151 and the Plainfield Distribution Center (PDC).

## References

- Global Records Retention Schedule
- USD-SOP-Facility State Licensing-039
- USD-SOP-Quarantine of Finished Products-001
- USD-SOP-Segregation Process and Scrapped Mtl Processing-001
- USD-SOP-Product Recpt at PDC-134
- Lilly Quality System (LQS) Glossary

## Reasons for Revision

To remove reference to GBIP-00596, WM Receiving Production. Rationale: It is not used in NALO.

## Definitions

- **Controlled Room Temperature** – See the LQS Glossary.
- **Chill Room (e.g., Refrigerator)** – A cold place in which the temperature is maintained thermostatically between 2° and 8° C (36° and 46° F).

## Rules & Restrictions for Movement of Material

1. Materials must be moved on temperature-controlled trucks with refrigeration unit on trailer setpoint at 42°F continuous mode for all product. A setpoint of 70°F may be used for loads containing CRT products only.
2. Material must be placed on pallets designated as heat treated in accordance with ISPM 15 requirements ("HT") and secured for shipping (banding, shrink wrap) prior to moving material to appropriate staging area.
3. Vault material must be capped and strapped, or stretch wrapped (with opaque wrap) and must bear no external markings identifying the contents as Special Security Substances.
4. Miscellaneous items may be shipped on the same truck with approved finished drug product providing: space is available on the truck, the item has a completed Physical Transfer Form attached, and the item does not pose a risk to the rest of the load.

## Material Arrival at PDC

> [!note]
> All steps are performed by PDC personnel unless noted otherwise. Driver contacts Dispatcher if truck is staged at PDC prior to seal being broken; Dispatcher provides tracking number and notifies monitoring firm of staging.

| Step | Action |
|---|---|
| 1 | Upon arrival, verify the Seal Number on the Bill of Lading matches the truck seal. Verify that the seal is intact. If it is not intact, notify NALO Quality Assurance. |
| 2 | Driver contacts Dispatcher upon the seal being broken upon arrival. |
| 3 | Dispatcher contacts monitoring firm and notifies them of trip completion. |
| 4 | PDC personnel and/or Delivery Services personnel unload the material at PDC dock. |
| 5 | Visually examine each pallet and contents of partial cases to ensure that product, containers, labeling are not suspected of being damaged, adulterated, misbranded, counterfeited, contraband, or otherwise unfit for distribution. |
| 6 | Perform receipt processing in accordance with USD-SOP-Product Recpt at PDC-134. |
| 7 | After receipt processing is complete, turn over contents of Distribution Receipt and/or packlist to NALO QA. |

> [!warning]
> If there is evidence of adulteration, misbranding, counterfeiting or suspicion of counterfeiting, notify NALO QA immediately. NALO QA will immediately impound the product per USD-SOP-Quarantine of Finished Products-001 and perform notifications per USD-SOP-Facility State Licensing-039. If product is damaged or the sealed outer/secondary containers have been opened or used, but there is no adulteration/misbranding/counterfeiting concern, notify NALO QA — product may be scrapped per USD-SOP-Segregation Process and Scrapped Mtl Processing-001 or impounded per USD-SOP-Quarantine of Finished Products-001.

## Returning Material to Building 151 from PDC

> [!note]
> All steps are performed by PDC personnel unless noted otherwise.

| Step | Action |
|---|---|
| 1 | Identify the material, batch, and quantity to be returned. |
| 2 | Schedule a temperature-controlled truck through Delivery Services (317-276-7765). |
| 3 | Ensure refrigeration unit on trailer setpoint is 42°F continuous mode for all product (70°F allowed for CRT-only loads). |
| 4 | Notify QA personnel to move the batch to Restricted Stock using SAP transaction MSC2N. |
| 5 | Use transaction VLMOVE with Movement Type 303 (Domestic) or 313 (Export). Enter material, batch, source plant, and storage location. |
| 6 | Click "Target Mat. Entry" tab. Enter destination plant and storage type (domestic VLMOVES: plants 0004, 0314, 0413, or 0350; export uses same plant as source). Enter quantity, click "Create Delivery", note the delivery number, go to VL03N and verify ship-to address/quantities. Wave, pick, and pack the outbound delivery in EWM. For serialized products: execute EWM RF Menu Management → Select Outbound → EWM RF Pick and Pack by Warehouse Order. |
| 7 | Obtain an Information Shipment Envelope and record: Date, Time, Seal #, Outbound Delivery #, refrigeration unit setpoint, Initial/Date. |
| 8 | PDC personnel and/or Delivery Services personnel load the material on the truck. |
| 9 | Attach Information Shipment Envelope to the last pallet in the truck. Complete the Bill of Lading. |
| 10 | Shut the door and attach the security seal. |
| 11 | Initiate load with the security firm monitoring the truck. |
| 12 | Post Goods Issue the deliveries that were loaded into the trailer. |

> [!note]
> If product that has not been PGR at PDC needs to be sent back to 151 due to a quality reason, ensure it is sent back on a Physical Transfer Form. Once the outbound is "goods issued," it automatically creates an inbound delivery for the receiving site.

---
*Source: USD-SOP-Mtl Mvt btw 151 and PDC-001, Version 17.0, Effective 18 Jan 2025. Approved 14 Jan 2025. Converted from PDF for reference — always verify current version in QualityDocs before relying on this for compliance decisions.*
