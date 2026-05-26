---
date: 2026-05-26
status:
type:
tags:
---
Here’s a **concise, executive-style summary** of the meeting **“Internal Huddle: P44 & Everstream Sharing”** (based on the transcript):

---

# 📌 **Meeting Summary**

## **🔑 Why this matters**

- Lilly is building **end-to-end supply chain visibility** using two core platforms:
    - **Everstream** → risk monitoring (facilities, suppliers, geopolitical events)
    - **Project44 (P44)** → real-time shipment visibility (in transit)
- The discussion focused on **how these tools complement each other**, and the **data/integration strategy needed to make them actionable**

---

## **🧠 Key Takeaways (What’s happening)**

### 1) Clear division of roles between platforms

- **Everstream**
    - Monitors **static network**: sites, suppliers, ports, warehouses
    - Provides **risk/event alerts** (e.g., geopolitical, natural disasters)
- **Project44 (P44)**
    - Tracks **live shipments in motion**
    - Pulls data from IoT devices, carriers, and transport systems
    - Links disruptions to **specific shipments and POs in real time**

👉 Bottom line:

- Everstream = **“What risks exist?”**
- P44 = **“What shipments are impacted right now?”**

---

### 2) Integration currently is limited and indirect

- P44 **ingests a global Everstream event feed**, not Lilly-specific configuration
- Events are **overlaid onto Lilly shipments**, but:
    - Not tailored to Lilly’s curated Everstream setup
    - No direct “closed-loop” integration yet

👉 Implication:

- Current setup is **useful but not fully optimized for Lilly-specific insights**

---

### 3) Scope focus (near-term vs future)

- **Current focus**
    - Outbound shipments (finished goods + API movements)
- **Future scope**
    - Inbound logistics (suppliers → manufacturing)
    - Full end-to-end network integration

👉 Dependency called out:

- Progress tied to **regional transportation hub and TMS infrastructure**

---

### 4) Major gap: Data & integration strategy

A key internal discussion highlighted:

- There is **no fully defined enterprise integration architecture yet**
- Open questions remain:
    - Where should data live? (SAP, data lake, P44, Everstream)
    - Who owns the data? (strong push: **Lilly must own it**)
    - How systems should connect (APIs, TMS, 4PL, etc.)

👉 Risk:

- Without alignment, tools may remain **fragmented and underutilized**

---

### 5) Shift needed: Use-case driven design

Leadership emphasized:

- Don’t start with tools → **start with user needs**
- Key questions:
    - What do we need to know?
    - When do we need to know it? (real-time vs long-range)
    - Where should users access it?

Examples:

- Site planners → prefer **SAP/Fiori integration**
- Leadership / crisis response → prefer **direct P44 visibility links**
- Risk teams → likely rely more on **Everstream**

---

### 6) Strong opportunity: Combined value

When integrated properly:

- Everstream identifies **potential disruptions early**
- P44 shows **exact shipment impact**
- Enables:
    - Proactive rerouting decisions
    - Better coordination across logistics and planning
    - Faster response during crises (e.g., geopolitical events)

---

## **⚠️ Key Challenges Identified**

- Lack of **clear prioritization across ~8+ initiatives**
- Unclear:
    - TMS rollout strategy (especially for smaller sites & suppliers)
    - Whether Lilly vs 4PL will own shipment data
- Risk of:
    - Multiple disconnected systems
    - Users needing to check multiple platforms

---

## **✅ Key Decisions / Direction**

- Focus should shift to:
    - **User stories and priority capabilities**
    - Not just system integration mechanics
- Likely future model:
    - **Dual experience**
        - Site-level: SAP/Fiori
        - Network/global: P44
- Strategic principle reinforced:
    - Lilly should aim for **single source of visibility + owned data**

---

## **📅 What’s next**

- Upcoming session with **Everstream team** to review integration from their side
- Potential follow-up session with:
    - Internal stakeholders
    - P44 team (if needed)
- Need for:
    - **Clear prioritization and roadmap alignment**
    - Definition of **integration architecture and ownership**

---

## **💡 Bottom line**

- The organization has **powerful tools in place**, but:
    - Value will depend on **integration, prioritization, and user-centered design**
- Biggest unlock: 👉 Connecting **risk signals (Everstream)** with **real-time execution (P44)** in a unified, usable way

---

If you want, I can also convert this into a **Smart Brevity email draft** or **exec-ready slide** for sharing with your team.

## Provide your feedback on BizChat