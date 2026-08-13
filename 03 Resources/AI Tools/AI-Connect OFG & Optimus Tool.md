---
date:
status:
tags:
  - type/meeting
  - initiative/ai-tools
  - initiative/optimus
---

# Purpose of the meeting

- Align on how **AI tools and artifacts** are being built and used to support **strategic supply chain planning**, capacity visibility, and scenario analysis.
- Share approaches between **Pablo’s OFG AI tool** and **Steven’s internal data + visualization tool**, and discuss how these may eventually align with **Kinaxis / River Logic / Maestro**.

## Key topics & discussion

### 1. Pablo’s OFG AI tool (strategic planning focus)

- Built to capture **3+ years of institutional knowledge** and evolving assumptions (sites, nodes, volumes, markets, constraints).
- Uses:
    - Curated **Projects** with scoped data to reduce hallucinations.
    - Static sources (PDFs, process flows, conversion factors) + dynamic sources (SharePoint decks updated weekly/monthly).
- Designed for **high‑level decision support**, not site‑level execution.
- Supports:
    - Capacity comparisons (e.g., 20M patient case vs 2024 plan).
    - Scenario reasoning (delaying nodes, reshuffling supply).
    - Visual supply chain maps with hover‑level detail (sites, markets, volumes, timelines).
- Distributed as a **shareable artifact** (GSX-based), not HTML, due to cybersecurity constraints.
- Known limitation: **No version control** → new link shared with each refresh (mitigated with “current as of” date stamps).

### 2. Steven’s internal tool & dataset (more granular / execution‑adjacent)

- Focuses on:
    - **Node-level throughput**, by product, material, site, and year.
    - Production, inventory, and demand nodes (not SKU‑level, but “bucketed” formats).
- Data sources:
    - Throughput collected directly from sites (not reliable in SAP today).
    - Forecast demand out to ~2030.
- Capabilities demonstrated:
    - Interactive supply chain maps.
    - Identification of **single points of failure**, utilization constraints, and unmet demand.
    - Scenario exploration (e.g., changing operating hours).
- Currently:
    - Optimizing **drug product only** (API assumed unconstrained).
    - Used experimentally alongside Optimus, not positioned to replace enterprise tools.

### 3. Alignment with enterprise systems (Kinaxis, River Logic, Maestro)

- Shared understanding:
    - **Long‑term optimization** belongs in River Logic.
    - Kinaxis / connected systems may be better for **heuristic or near‑term planning**.
- Open questions:
    - Where future nodes should be built first (connected vs River Logic).
    - How to ensure throughput assumptions stay consistent across tools.
- Mentioned related work:
    - **David & Kamiya’s supply chain mapping effort** (visualizing Maestro data).
    - Potential future state: digital twin with node history, effectivity dates, and audit trail.

## Decisions & agreements

- Pablo’s AI tool is **strategic, explanatory, and forward‑looking**, not a replacement for formal optimization.
- Steven’s dataset is valuable primarily for **equipment‑level throughput realism**.
- Optimization should **not diverge** from official tools, but exploratory outputs may be used for comparison and insight.
- Naming conventions and data structures should be checked with **Arturo** to align with River Logic best practices.

## Next steps (explicitly stated in the meeting)

- **Steven & Pablo** to meet for ~1 hour on **Wednesday** to:
    - Review newly added products.
    - Validate throughput and demand data.
- Steven to:
    - Review naming conventions with **Arturo**.
    - Continue data validation before engaging further with IT (Robert) on optimization.
- Pablo to:
    - Share an **overview video** of the existing supply chain mapping work (Kinaxis-based).
- Team to keep River Logic timelines in mind (June mentioned, but uncertain).

## Big picture takeaway

The group is converging on a **layered approach**:

- AI artifacts → fast, explainable, strategic insight and storytelling.
- Curated datasets → realistic throughput and constraints.
- Enterprise tools → formal optimization and execution.