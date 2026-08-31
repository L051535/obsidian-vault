# 📋 ZS163N Data Review Meeting Recap (So Far)

**Meeting Status:** In Progress  
**Transcript Available:** Yes (summary below is paraphrased from the meeting transcript)

# 👥 Attendees / Participants

### Meeting Speakers

- Steven Chen
- Rich Zeinner
- Arturo Garcia
- Rajavelu Kandasamy

### Additional Meeting Chat Participant

- Alex Parker (indicated via meeting chat that they were double booked and unable to attend)

# 🎯 Executive Summary

The discussion centered on the future viability of the **ZS163N report/transaction** following the **TPQM implementation**. The team learned that a significant portion of the supplier-related information currently surfaced through ZS163N will move into **Viva**, making SAP-only reporting incomplete.

As a result, the team evaluated alternative approaches for obtaining manufacturing and vendor information needed for risk analysis. The preferred direction is to use SAP base tables, particularly:

- **QINF (Quality Info Records)**
- **LFA1 (Vendor Master Data)**

and potentially establish a **Data Product** instead of continuing to rely on the ZS163N-derived solution.

The discussion also covered how this data supports vendor/manufacturer risk monitoring, including site-level disruption analysis, supply chain exposure, and material sourcing diversification.

# 🔑 Detailed Key Takeaways

## 1. ZS163N Will Become Incomplete After TPQM Changes

Rich explained that major changes are coming to ZS163N because TPQM will move part of the underlying supplier/manufacturer information into Viva.

Key impacts:

- Not all supplier criticality information will remain in SAP.
- SAP classification fields alone will no longer provide a reliable picture.
- Some critical supplier information will require cross-referencing with Viva.
- Existing ZS163N-based solutions may become obsolete or incomplete after go-live.

## 2. QINF + LFA1 Identified as the Preferred SAP Alternative

Rich recommended moving away from creating extracts from ZS163N and instead using SAP source tables.

### QINF

Provides:

- Material approvals
- Plant-specific quality information
- Manufacturer relationships

### LFA1

Provides:

- Vendor master information
- Vendor addresses
- DUNS numbers
- Additional supplier details

The tables can be connected through vendor identifiers to create a cleaner and more sustainable solution.

## 3. MP Vendor Data Remains Important

Arturo clarified that the primary business need is identifying:

- The actual manufacturing facilities
- Original manufacturers (MP Vendors)
- Geographic manufacturing locations
- Vendor diversification

Rich confirmed that the original manufacturer information should still be available through the QINF-based approach and can be enriched using LFA1.

## 4. Risk Management Is the Main Business Use Case

Arturo provided extensive business context around how the data is used.

The organization uses supplier/manufacturer information to:

- Monitor supply chain risks
- Assess vendor concentration exposure
- Track manufacturing site locations
- Evaluate disruption events

Examples discussed included:

- Floods
- Facility disruptions
- Workforce reductions
- Geographic events affecting supplier manufacturing sites

The supplier-location data enables external risk-monitoring tools to identify events and assess potential impacts on materials and plant operations.

## 5. Data Product Approach Preferred Long Term

Rich stated that a formal **Data-as-a-Product** solution appears to be the preferred strategic solution.

Discussion points:

- Similar products already combine multiple SAP tables.
- QINF + LFA1 integration should be feasible.
- Validation is needed to determine whether LFA1 is already available in the data platform.
- If unavailable, ingestion work will be required.

## 6. GMDF / Refined Layer Availability Needs Verification

Rajavelu reviewed current availability in the refined layer and reported:

- **LFA1 does not appear to be available**
- QINF status requires further verification
- New ingestion requests may be required

Potential next steps discussed:

1. Ingest missing SAP tables
2. Promote required data through Raw → Refined layers
3. Build a Data Product
4. Make data available within GMDF-DP

## 7. Two Parallel Paths Agreed

The group agreed to pursue two paths simultaneously:

### Path A

Create or request a formal Data Product using required SAP source tables.

### Path B

Investigate whether the required tables already exist and can be joined directly to satisfy the business needs more quickly.

This approach allows progress while enterprise data product work proceeds.

# ✅ Decisions Made

|Decision|Outcome|
|---|---|
|Continue relying on ZS163N|❌ No, not preferred due to TPQM impact|
|Use QINF + LFA1 approach|✅ Preferred technical direction|
|Investigate Data Product creation|✅ Yes|
|Check refined layer availability|✅ Yes|
|Pursue multiple solution paths|✅ Yes|
|Submit ingestion requests if tables missing|✅ Agreed|

Source: Meeting transcript

# 📌 Action Items

|#|Action Item|Owner|Status|
|---|---|---|---|
|1|Verify whether LFA1 exists in GMDF refined layer and confirm QINF availability/status|**Rajavelu Kandasamy**|Open|
|2|Submit ingestion request for LFA1 if not already available|**Rajavelu Kandasamy**|Open|
|3|Determine whether QINF requires ingestion and/or promotion to refined layer|**Rajavelu Kandasamy**|Open|
|4|Include required tables within GMDF-DP intake/process scope|**Rajavelu Kandasamy** and **Arturo Garcia**|Open|
|5|Validate whether LFA1 is already included within existing data product architecture|**Rich Zeinner**|Open|
|6|Assist with QINF/LFA1 joins, table relationships, and SAP data modeling guidance|**Rich Zeinner**|Open|
|7|Explore formal Data Product creation using QINF and LFA1|**Rich Zeinner / GMDF Team**|Open|
|8|Evaluate direct-table approach as a parallel option while Data Product work progresses|**Arturo Garcia**|Open|
|9|Investigate DIA/Todd's solution and determine what source data it uses|**Rajavelu Kandasamy** (follow-up with Todd's team)|Open|
|10|Assess whether business requirements can be fulfilled through QINF + LFA1 instead of ZS163N|**Arturo Garcia, Steven Chen, Rich Zeinner**|Open|

Source: Meeting transcript discussion and explicit ownership references

# 🏁 Closing Alignment

The meeting concluded with agreement that:

1. **ZS163N should no longer be the primary long-term solution.**
2. **QINF + LFA1 appears to be the most promising replacement approach.**
3. **Rajavelu will investigate data availability and ingestion requirements.**
4. **Rich will help with SAP table relationships and Data Product discussions.**
5. **Arturo will evaluate how either approach satisfies the downstream risk analytics use cases.**