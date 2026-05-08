---
date: 2026-05-08
status:
type:
tags:
---
# Material Group → Commodity Code Description: Process Logic

## Purpose

Translate each row's Material Group code into a human-readable commodity description, using a master procurement taxonomy as the reference. As part of the process, legacy alphanumeric MGC codes are normalized into their numeric L1 Commodity Code equivalents so the working dataset uses a single, consistent code system.

---

## Inputs

1. **Working dataset (ZS163n)** — the transactional records, where each row carries a Material Group code that needs to be described.
2. **Master taxonomy** — the authoritative hierarchy of procurement codes. It defines three levels (L1 → L2 → L3) and a parallel legacy code system (MGC), with a name/description for each.
3. **MGC-to-L1CC crosswalk** — a small lookup table mapping each legacy MGC code to its modern numeric L1 Commodity Code.

---

## Code-format conventions

- The master taxonomy stores codes with separators (e.g., level segments joined by dashes).
- The working dataset stores them without separators.
- **Code length signals hierarchy depth:**
    - 3 characters → L1 (top level)
    - 5 characters → L2 (mid level)
    - 7+ characters → L3 (most specific)
- **MGC codes** are 3-character alphanumeric (begin with a letter) and represent the legacy system being phased out.

---

## Step 1 — Normalize the master taxonomy's lookup key

The master taxonomy's codes carry separators while the working dataset's codes don't. Build a parallel "stripped" version of the master code — the same code with separators removed — so that values from the two sources can be compared directly.

This stripped key becomes the join column for all subsequent lookups.

---

## Step 2 — Convert legacy MGC codes in the working dataset to L1 Commodity Codes

Some rows in the working dataset still carry legacy MGC codes instead of the numeric scheme. Normalize them so the column holds one code system.

1. **Freeze the Material Group column.** If it was being computed by a formula referencing another sheet, replace those formulas with their current static values. This locks the codes in place so the next step can rewrite them safely.
2. **Apply the crosswalk.** For each row, check whether the Material Group value matches an entry in the MGC-to-L1CC crosswalk. If it does, replace it with the corresponding numeric L1 Commodity Code. If it doesn't match, leave it as-is (it's already a numeric code).

After this step, every Material Group value is a numeric code of length 3, 5, or 7+.

---

## Step 3 — Resolve each code to a description using a cascading lookup

For each row, decide which level of the taxonomy to consult based on the code's length, then walk **up** the hierarchy if the most specific level doesn't yield a usable description.

### Lookup priority by code length

|Code length|Try in this order, stopping at the first usable result|
|---|---|
|3|L1 description → MGC description|
|5|L2 description (use full code) → L1 description (use first 3 chars) → MGC description (use first 3 chars)|
|7+|L3 description (full code) → L2 description (first 5) → L1 description (first 3) → MGC description (first 3)|

### What counts as "not usable"

A lookup result is treated as a miss — and the cascade falls through to the next-higher level — when **either**:

- The code isn't found in that level at all, **or**
- The taxonomy returns the placeholder text `"Default"`, which the master sheet uses to mark cells where a given level doesn't apply (e.g., a row that defines an L2 but has no real L3 underneath it).

### Why the cascade matters

A 7-digit code might exist in the taxonomy only down to L2 — its L3 row says `"Default"`. Without the cascade, the result would be the literal string `"Default"`, which is meaningless. With the cascade, the row falls back to the L2 description, which is the most specific real name available.

The same logic applies one level up: if L2 is also `"Default"` or missing, fall back to L1; if L1 is missing, fall back to the legacy MGC description as a last resort.

---

## Step 4 — Verification

Spot-check a representative sample covering each branch of the logic:

- A 3-character code that exists in L1 → confirm L1 description is returned.
- A 5-character code where L3 is irrelevant — confirm L2 description is returned (not `"Default"`).
- A 7+ character code with a real L3 entry → confirm L3 description is returned.
- A code where the deepest level is `"Default"` → confirm the cascade falls back to the next-higher real description.
- A code that doesn't appear at any L1/L2/L3 → confirm it resolves via MGC, or returns blank if nothing matches.

---

## Notes for future maintainers

- **Adding new taxonomy entries:** add the new row to the master taxonomy with the separator-formatted code; the stripped-key helper picks it up automatically. Only new legacy codes require updating the MGC-to-L1CC crosswalk.
- **Refreshing the working dataset:** if the Material Group column is rebuilt from a fresh source (e.g., re-pulled from SAP), repeat Step 2 to re-normalize legacy MGCs before relying on the descriptions.
- **Treat `"Default"` as a sentinel, not a value.** Any logic that consumes the master taxonomy must skip `"Default"` results the same way this process does.
- **Code length is the dispatcher.** Any change that introduces a new code length (e.g., a 4-character or 9-character scheme) requires extending the cascade rules to handle it explicitly.