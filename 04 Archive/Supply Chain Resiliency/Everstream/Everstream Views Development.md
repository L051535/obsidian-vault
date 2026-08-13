---
date: 2026-04-02
status:
tags:
  - initiative/everstream
  - SC-Resiliency-Sustainability
  - type/reference
---
# Important Views

**1.  (New) High Impact & Severity
- Contains **NEW** high severity incidents that have at least 1 impacted site and with high severity defined by Everstream
- *Filters:*
	- Facility Impact Count >= 1
	- ~={red}Severity: Severe, Extreme=~
	- Triaging Status = "To Do"

**2. (Assessing) High Impact & Severity**
- Contains high severity incidents that we have started looking at already
- *Filters:*
	- Facility Impact Count >= 1
	- ~={red}Severity: Severe, Extreme=~
	- Triaging Status = "Assessing"
	
**3. (Resolved/Closed) High Impact & Severity**
- Contains high severity incidents that we have decided are resolved or no longer important
- *Filters:*
	- Facility Impact Count >= 1
	- ~={red}Severity: Severe, Extreme=~
	- Triaging Status = "Resolved/Closed"

**4.  (New) Moderate Severity**
- Contains **NEW** moderate severity incidents that have at least 1 impacted site and with high severity defined by Everstream
- *Filters:*
	- Facility Impact Count >= 1
	- ~={yellow}Severity: Moderate=~
	- Triaging Status = "To Do"

**5. (Assessing) Moderate Severity**
- Contains moderate severity incidents that we have started looking at already
- *Filters:*
	- Facility Impact Count >= 1
	- ~={yellow}Severity: Moderate=~
	- Triaging Status = "Assessing"
	
**6. (Resolved/Closed) Moderate Severity**
- Contains moderate severity incidents that we have decided are resolved or no longer important
- *Filters:*
	- Facility Impact Count >= 1
	- ~={yellow}Severity: Moderate=~
	- Triaging Status = "Resolved/Closed"

**7. All Incidents
- Unfiltered version containing all incidents tracked/untracked.
- Default is showing only **Active** events, but we can toggle that in the **"Settings"** modal


## Related Tags:
