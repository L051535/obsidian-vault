---
date: 2026-04-30
status:
type:
tags:
---
## Meeting Summary (Smart Brevity)

### Big Picture

The discussion focused on **how to scope, maintain, and operationalize Everstream facility data and risk scoring**—especially for **global logistics nodes (warehouses, distribution centers, ports, airports)**—while avoiding unsustainable manual processes.

### Key Topics & Decisions

**1. Facility Data Scope & Quality**

- Current Everstream dataset (~4,000 facilities) includes:
    - SAP-sourced facilities (majority)
    - Manually added sites (logistics nodes, under-evaluation sites, TPRM-related facilities)
- Manual additions are creating **data lineage and maintenance challenges**
- Clear need to:
    - Validate what legacy Resilience/ResLink data should remain
    - Identify **gaps** and questionable entries (e.g., “what is this facility actually?”)
    - Improve transparency on **data sources and refresh ownership**

**Decision:**  
Focus on **stabilizing and documenting current facility data** before expanding scope.

**2. Logistics-Specific Use of Everstream**

- Everstream is seen as valuable for **real-time incident monitoring** for:
    - Ports
    - Airports
    - Warehouses / DCs
- Current issue: many logistics nodes are ranked equivalently, reducing insight
- Discussion around whether to:
    - Keep Everstream’s **standard strategic risk scores**
    - Customize scoring only if it’s clearly valuable to logistics users

**Decision:**  
**Defer custom risk scoring for logistics facilities** until users demonstrate a clear need and defined methodology.

**3. Manual Data Maintenance – Core Risk**

- Manual updates (Excel uploads, ad hoc sources) are:
    - Hard to trace
    - Hard to sustain with attrition
    - Not easily auditable
- Strong concern about building more capability on top of unclear ownership

**Direction:**

- Treat **manual facility maintenance** as a defined scope item
- Potentially support with:
    - A controlled app/form (vs open Excel editing)
    - Outsourced support (TCS / Infosys / Bureau), if requirements are clearly defined

**4. Operating Model: Empower, Don’t Centralize**

- Goal is to **enable zone leaders, logistics, affiliates** to:
    - See relevant risks in real time
    - Act locally
    - Escalate only when thresholds are met
- Avoid turning resiliency into a “central hotline” or constant alerting function

**Expectation to Users:**

- If they have direct access and see a severe/extreme event:
    - They either confirm “no impact”
    - Or escalate with context
- In return, they help maintain relevance of logistics facility data

**5. Recommended Roadmap (High-Level)**

1. **Stabilize facility dataset**
    - Document data sources
    - Define refresh cadence & owners
    - Resolve “unknown” facility entries
2. **Pilot with Global Logistics**
    - Limited, high-value scope
    - Training on filters & user profiles
3. **Manual data maintenance solution**
    - Controlled intake (app or governed process)
4. **Future (optional)**
    - Revisit logistics-specific risk scoring
    - Explore automation / API with IT

### Open Follow-Ups

- Validate logistics-relevant risk categories (e.g., theft)
- Confirm whether internal theft data (BSI / Sensiguard) should supplement Everstream
- Align on who owns:
    - Manual facility entries
    - Data documentation
    - Ongoing refresh

If you want, I can **turn this into**:

- A **1-slide leadership recap**
- A **follow-up email** to logistics stakeholders (Smart Brevity)
- A **draft roadmap visual** similar to what was sketched live