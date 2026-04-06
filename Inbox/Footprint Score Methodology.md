---
date: 2026-04-06
status:
type:
tags:
  - initiative/supplier-of-focus
---
### How to Calculate

**Footprint Score (1–10):** How embedded the supplier is across the organization

This one needs a bit more thought. Three natural dimensions:

|Dimension|What it captures|
|---|---|
|# of Plants served|Geographic / operational spread|
|# of Materials supplied|Breadth of dependency|
|# of MP Vendors linked|How many sourcing relationships flow through them|

I'd recommend a **percentile-rank approach** for footprint rather than an absolute scale — because the distributions are heavily skewed (most suppliers serve 1–3 plants, but a few serve 100+). Raw counts wouldn't differentiate well.

`Footprint Score = Percentile rank among all suppliers × 10`

You could use a single dimension (e.g., # of unique Material × Plant combinations) or a composite. The simplest and most meaningful is probably **# unique materials × plants served**, since that captures both breadth and spread in one number.