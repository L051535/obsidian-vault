I found a previous NALO capacity-planning effort that aligns very closely with what you are trying to build. The project charter's objective was to create **an interactive tool to model the operating day, test sensitivity, forecast scenarios, define functional area capacities, and model staffing/overtime impacts**. It specifically called out SLA requirements, dock cutoffs, asset utilization, storage capacity, labor needs, and ASRS capacity as key inputs. [[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[Project Ch...h Appendix | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B9353AF8F-4666-4E58-A3D9-A8F3B676AAA2%7D&file=Project%20Charter%20-%20NALO%20Capacity%20with%20Appendix.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

Based on that NALO approach and warehouse capacity-planning best practices, I would recommend building your model in **five layers**:

# NALO PDC Capacity Model Framework

## Layer 1: Demand Model

This is your workload forecast.

### Inputs

- Orders/day
- Order lines/day
- Eaches/day
- Pallets received/day
- Pallets shipped/day
- Export loads/day
- Sample orders/day
- Seasonal peaks
- New product launches
- Network transfers

### Outputs

- Average day
- Busy day
- Peak day
- Future-state volume scenarios (1 year, 3 year, 5 year)

**NALO Examples**

- Base demand
- +10% volume growth
- +20% volume growth
- New manufacturing site onboarding
- ASRS shutdown scenario
- Launch surge scenario

---

# Layer 2: Storage Capacity Model

This answers:

**"Can we physically store the inventory?"**

### Measure by Storage Type

|Area|Capacity Driver|
|---|---|
|ASRS|Pallet positions|
|CRT|Pallet positions|
|Ambient Rack|Pallet positions|
|Chilled|Pallet positions|
|Export Staging|Staging positions|
|Receiving|Staging positions|

### Calculations

```
Current Inventory
/
Available Positions
=
Storage Utilization %
```

Example:

```
18,500 pallets stored
20,000 pallet positions

= 92.5% utilized
```

### Key Thresholds

|Utilization|Status|
|---|---|
|<80%|Healthy|
|80-90%|Watch|
|90-95%|Capacity Risk|
|>95%|Critical|

---

# Layer 3: Throughput Capacity Model

This is typically the most important model.

It answers:

**"Can we process the incoming work?"**

Measure each major function separately.

### Receiving

```
Trucks/day
Pallets/day
Cases/day
```

### Putaway

```
Pallets/hour
```

### Picking

```
Eaches/hour
Cases/hour
Lines/hour
```

### Packing

```
Orders/hour
```

### Shipping

```
Pallets loaded/hour
Truck loads/day
```

### Export

```
Loads/day
```

### Sample Operations

```
Orders/day
```

---

## Functional Capacity Formula

For every area:

```
Capacity
=
Resources
×
Productivity
×
Available Hours
```

Example:

```
8 operators

x

55 pallets/hour

x

8 hours

=

3,520 pallets/day
```

---

# Layer 4: Labor Capacity Model

Build staffing requirements directly into the model.

### Inputs

- Headcount by function
- Shift structure
- Overtime %
- PTO assumptions
- Training availability
- Cross-training capability

### Output

Required headcount:

```
Workload
/
Productivity
=
Labor Hours Needed
```

```
Labor Hours Needed
/
Hours Available
=
Headcount Required
```

---

## Overtime Sensitivity

I would model exactly what the previous NALO charter proposed:

- 0% OT
- 5% OT
- 10% OT
- Current State

[[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[Project Ch...h Appendix | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B9353AF8F-4666-4E58-A3D9-A8F3B676AAA2%7D&file=Project%20Charter%20-%20NALO%20Capacity%20with%20Appendix.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

Example:

|Volume|Staff Needed|OT Required|
|---|---|---|
|Current|50|0%|
|+10%|55|4%|
|+20%|61|11%|
|+30%|68|18%|

---

# Layer 5: Constraint Model

This becomes the "digital twin" of PDC.

Identify every major bottleneck.

### Constraints

- ASRS capacity
- CRT capacity
- Chilled storage
- Dock doors
- AGVs
- Forklifts
- Export processing
- Quality release timing
- Transportation pickup windows
- Labor availability

For every scenario ask:

**Which constraint breaks first?**

Example:

|Scenario|Constraint|
|---|---|
|+10% Growth|Picking labor|
|+20% Growth|Ship dock|
|+30% Growth|ASRS storage|
|ASRS Down|CRT storage|

---

# Recommended Dashboard Outputs

I would build a Power BI or Excel dashboard showing:

## Executive View

- Storage utilization %
- Throughput utilization %
- Labor utilization %
- Overtime %
- Capacity remaining %
- Forecast date capacity exhausted

---

## Warehouse View

By area:

|Area|Utilization|
|---|---|
|Receiving|72%|
|ASRS|88%|
|Pick Module|93%|
|Shipping|77%|
|Export|69%|

---

## Scenario Planning

Buttons or filters for:

- Current State
- Peak Season
- New Product Launch
- ASRS Outage
- +10% Growth
- +20% Growth
- +30% Growth

---

# NALO PDC-Specific Metrics I Would Include

Based on the PDC operation and prior NALO capacity work:

- Pallet positions by storage type
- CRT occupancy
- ASRS occupancy
- Eaches processed/day
- Order lines/day
- Trucks/day
- Export loads/day
- Labor hours/day
- Overtime %
- Cost per each shipped
- Throughput (eaches/hour)
- Dock utilization
- Inventory days on hand
- 3PL overflow volume

These align closely with the capacity objectives already documented in the NALO capacity charter and site planning work. [[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[Project Ch...h Appendix | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B9353AF8F-4666-4E58-A3D9-A8F3B676AAA2%7D&file=Project%20Charter%20-%20NALO%20Capacity%20with%20Appendix.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[NALO Site...Plan 2030 | PowerPoint]](https://collab.lilly.com/sites/NALOLEADTEAMPROJECTPRIORITIZATION/_layouts/15/Doc.aspx?sourcedoc=%7B579168E6-E40A-4B62-8717-FDEC8525B9FF%7D&file=NALO%20Site%20Master%20Plan%202030.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

**My recommendation:** Start with a simple Excel model using **Demand → Storage → Throughput → Labor → Constraints**. Once validated, move it into Power BI so supervisors and leadership can run growth and ASRS-risk scenarios themselves.

##### You said:

Build a sample NALO PDC scenario analysis

###### Copilot said:

Below is a **sample NALO PDC scenario analysis** that you could use to demonstrate the value of a capacity model to Pete, Ivan, Russell, or leadership. The structure follows the objectives identified in the NALO capacity planning project charter, including operating-day modeling, staffing sensitivity, utilization, and future-state forecasting. [[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[Project Ch...h Appendix | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B9353AF8F-4666-4E58-A3D9-A8F3B676AAA2%7D&file=Project%20Charter%20-%20NALO%20Capacity%20with%20Appendix.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

# NALO PDC Capacity Scenario Analysis

## Scenario Assumptions

### Current State (Baseline)

|Metric|Current State|
|---|---|
|Orders/Day|6,000|
|Order Lines/Day|18,000|
|Eaches/Day|150,000|
|Inbound Pallets/Day|450|
|Outbound Pallets/Day|650|
|PDC Storage Positions|20,000|
|Inventory Stored|18,200|
|Headcount|85|
|Overtime|13%|
|Operating Hours|16|

---

## Functional Capacities

|Function|Daily Capacity|
|---|---|
|Receiving|600 Pallets|
|Putaway|700 Pallets|
|Picking|175,000 Eaches|
|Packing|6,500 Orders|
|Shipping|750 Pallets|
|Export|80 Loads|

---

# Scenario 1 – Current State

### Results

|Area|Demand|Capacity|Utilization|
|---|---|---|---|
|Receiving|450|600|75%|
|Putaway|450|700|64%|
|Picking|150,000|175,000|86%|
|Packing|6,000|6,500|92%|
|Shipping|650|750|87%|
|Storage|18,200|20,000|91%|

### Findings

✅ Operation remains functional

⚠️ Storage nearing risk zone

⚠️ Packing is approaching bottleneck

⚠️ Shipping has limited surge capacity

---

# Scenario 2 – Peak Flu/Cough and Cold Season (+15%)

## Increased Demand

|Metric|New Volume|
|---|---|
|Orders|6,900|
|Eaches|172,500|
|Outbound Pallets|748|

### Results

|Area|Utilization|
|---|---|
|Receiving|86%|
|Putaway|74%|
|Picking|99%|
|Packing|106%|
|Shipping|100%|

### Findings

**Primary Constraints**

1. Packing
2. Picking
3. Shipping

### Consequences

- Missed carrier cutoffs
- Increased overtime
- Delayed order release
- Growing backlog

### Mitigation

- Add temporary labor
- Extend pack operations
- Cross-train CRT personnel
- Add weekend production

---

# Scenario 3 – New Manufacturing Site Launch (+25%)

### Assumptions

A new site begins feeding product into NALO, increasing inventory and throughput.

### Results

|Area|Utilization|
|---|---|
|Receiving|94%|
|Putaway|80%|
|Picking|108%|
|Packing|115%|
|Shipping|109%|
|Storage|96%|

### Findings

The operation becomes constrained in four areas:

- Storage
- Picking
- Packing
- Shipping

### Business Impact

- Overtime exceeds 20%
- Increased congestion in CRT
- Additional off-site storage required
- SLA risk begins to grow

---

# Scenario 4 – ASRS Storage Disruption

A scenario especially relevant to future NALO risk planning because leadership continues to discuss long-term storage and ASRS capacity needs. [[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[NALO Site...Plan 2030 | PowerPoint]](https://collab.lilly.com/sites/NALOLEADTEAMPROJECTPRIORITIZATION/_layouts/15/Doc.aspx?sourcedoc=%7B579168E6-E40A-4B62-8717-FDEC8525B9FF%7D&file=NALO%20Site%20Master%20Plan%202030.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

### Assumption

30% loss of ASRS storage capability.

### Impact

|Metric|Before|After|
|---|---|---|
|Available Positions|20,000|14,000|
|Inventory|18,200|18,200|
|Utilization|91%|130%|

### Findings

Critical failure point.

The operation cannot support current inventory levels.

### Required Actions

- Overflow to 3PL
- Cross-dock product
- Inventory reduction strategy
- Temporary storage trailers

### Leadership Question

"What is the daily cost of storing excess inventory outside PDC?"

The original NALO capacity project specifically called out understanding the cost of cross-docking and external storage impacts. [[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[Project Ch...h Appendix | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B9353AF8F-4666-4E58-A3D9-A8F3B676AAA2%7D&file=Project%20Charter%20-%20NALO%20Capacity%20with%20Appendix.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

---

# Scenario 5 – Overtime Elimination

One objective identified during prior NALO planning was analyzing staffing sensitivity at different overtime levels. [[Project Ch...O Capacity | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B907B1517-DD29-49E3-960C-1601CD5BC63F%7D&file=Project%20Charter%20-%20NALO%20Capacity.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [[Project Ch...h Appendix | PowerPoint]](https://collab.lilly.com/sites/NALOOperations-NALOCapacity/_layouts/15/Doc.aspx?sourcedoc=%7B9353AF8F-4666-4E58-A3D9-A8F3B676AAA2%7D&file=Project%20Charter%20-%20NALO%20Capacity%20with%20Appendix.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

## Current

- 85 employees
- 13% OT

### Calculated Capacity

```
85 x 8 hr x 1.13
=
768 labor hours/day
```

## Remove Overtime

```
85 x 8 hr
=
680 labor hours/day
```

### Gap

```
768 - 680
=
88 hours/day
```

### Additional Headcount Needed

```
88 / 8
=
11 FTE
```

### Finding

To eliminate 13% overtime, PDC would require approximately 11 additional operators unless productivity improvements are achieved.

---

# Executive Summary Slide

### Current State

- Storage Utilization: 91%
- Pick Utilization: 86%
- Pack Utilization: 92%
- Shipping Utilization: 87%

### First Constraint

✅ Packing

### Second Constraint

✅ Shipping

### Third Constraint

✅ Storage

### At +15% Growth

- Packing exceeds capacity
- Shipping reaches capacity
- Picking reaches capacity

### At +25% Growth

- Multiple bottlenecks created
- Additional labor required
- Additional storage required

### Strategic Recommendation

1. Track storage by type (ASRS, CRT, Chilled, Ambient)
2. Build labor model by functional area
3. Monitor picking and packing utilization weekly
4. Forecast capacity exhaustion by month
5. Use the model to evaluate future NALO growth, ASRS risk, and site onboarding scenarios

This provides the foundation for a true "NALO PDC Digital Twin" where leadership can test volume growth, inventory growth, staffing changes, ASRS outages, and launch scenarios before they impact operations.