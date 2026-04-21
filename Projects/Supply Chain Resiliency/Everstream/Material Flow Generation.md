---
date: 2026-04-21
status:
type:
tags:
---
# Material Flow Data — Build Instructions

## Objective

Generate rows for the `Material Flow Data` sheet (or a `(Generated)` copy) with these 12 columns:

1. **Material Id** — The material being sourced. Must exist in `Material Data`.
2. **From Asset Id** — Supplier location the material is sourced from. Must exist in `Facility Data`.
3. **To Material Id** — Downstream material/product the sourced material goes into. If direct/Tier-1, equals `Product Id`. Must exist in `Material Data`.
4. **To Asset Id** — Downstream location. If direct/Tier-1, equals `Production Asset Id`. Must exist in `Facility Data`.
5. **Product Id** — Final product sold to customers. Must exist in `Material Data`.
6. **Production Asset Id** — Production location of the final product. Must exist in `Facility Data`.
7. **Material Name** — Name from `Material Data` lookup on Material Id.
8. **From Asset Name** — Name from `Facility Data` lookup on From Asset Id.
9. **To Material Name** — Name from `Material Data` lookup on To Material Id.
10. **To Asset Name** — Name from `Facility Data` lookup on To Asset Id.
11. **Product Name** — Name from `Material Data` lookup on Product Id.
12. **Sourcing Type** — `Internal` if From Asset Facility name contains LILLY/LLY, else `External`.

## Source tabs & key columns

- **Material Data** — col A `Material or Product Id` (plant-prefixed, e.g. `0131_QA770D`), col B `Is Final Product` (`Y`/`N`), col C name.
- **Facility Data** — col A `Facility_ID`, col C `Name`.
- **bom_master_4_20_26** — col A `Plant`, col H `Material` (header, unprefixed), col L `Component` (unprefixed).
- **ZS163n_Filtered** — col B `Plant`, col D `Material`, **col H `MP Vendor`** (← this is the supplier, NOT col I `Vendor`).

## Scoping rule

Start with **finals where `Is Final Product = Y`**. Unless told otherwise:

- Run a **test batch of ~50** finals first, random across plants, restricted to those that have a BOM header match.
- Do not scale to all finals without explicit approval.

## Generation logic

**Step 1 — Tier 1 (direct suppliers of the final's components).** For each final `{plant}_{suffix}`:

- Look up BOM header: `bom_master` rows where Plant = `plant` AND Material = `suffix`. Collect the set of Components.
- For each Component, look up suppliers: `ZS163n_Filtered` rows where Plant = `plant` AND Material = `component`. Use **MP Vendor** (col H).
- Emit one row per (Component × MP Vendor):
    - Material Id = `{plant}_{component}` (prefer plant-prefixed form that exists in Material Data)
    - From Asset Id = normalized MP Vendor (see vendor rules)
    - To Material Id = final id
    - To Asset Id = final's plant
    - Product Id = final id
    - Production Asset Id = final's plant

**Step 2 — Tier 2 (sub-components).** For each Tier-1 Component that itself has a BOM:

- Get its sub-components and their MP Vendors.
- Emit one row per (Sub-component × MP Vendor):
    - Material Id = `{plant}_{subcomponent}`
    - From Asset Id = normalized MP Vendor
    - To Material Id = `{plant}_{component}` (the Tier-1 component)
    - To Asset Id = plant
    - Product Id = final id
    - Production Asset Id = plant

**Step 3 — ZS163n fallback (no BOM).** If a final has no BOM header match:

- Look up MP Vendors in `ZS163n_Filtered` for Plant + suffix directly.
- Emit one row per MP Vendor with Material Id = To Material Id = Product Id = the final itself, To Asset Id = Production Asset Id = final's plant.

## Vendor ID normalization

Facility Data sometimes omits a leading `P` that ZS163n carries for the same site.

- Try `facMap[vendor]` first.
- If no hit and vendor starts with `P`, retry `facMap[vendor without P]`.
- If hit, use the stripped form. Otherwise keep as-is (From Asset Name will be blank).

## BOM matching

- Material Data IDs are plant-prefixed (`0049_HI0319005ID`).
- BOM Material is unprefixed (`HI0319005ID`).
- Match on **Plant + Material suffix**, never on material alone.

## Output rules

- Write to a new sheet **`Material Flow Data (Generated)`** unless told to append/replace the main sheet. Do not overwrite existing `Material Flow Data` rows.
- Header row bolded, light-blue fill.
- **Dedupe** on the 6 ID columns (Material Id, From Asset Id, To Material Id, To Asset Id, Product Id, Production Asset Id).
- All 12 columns populated; name columns blank only when the ID has no lookup match.

## Clarifications to confirm up front

- **Existing rows**: replace / append / new sheet
- **Scope**: all Y finals / only Y finals with BOMs / small test batch
- **BOM match rule**: plant + material (default) vs material only
- **Vendor normalization**: strip leading P (default) vs keep as-is
- **Ad-hoc requests** (e.g. "generate for QA770D"): include all Y-flagged variants across plants; use BOM where available, ZS163n fallback otherwise. Confirm whether N-flagged variants should also be included.