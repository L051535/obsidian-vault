---
date: 2026-04-02
type: reference
status:
tags:
  - initiative/everstream
  - SC-Resiliency-Sustainability
  - type/reference
---
# Views I've Developed

**1. Tracked High Impact & Severity
- Contains relevant incidents that have at least 1 impacted site and with high severity defined by Everstream
- *Filters:*
	- Facility Impact Count >= 1
	- Severity: Severe, Extreme
	- Tracked = True

**2. Untracked High Impact & Severity**
- Contains incidents that we have looked at with the same as the tracked high severity version but have actively untracked/de-prioritized
- *Filters:*
	- Facility Impact Count >= 1
	- Severity: Severe, Extreme
	- Tracked = True

**3. Tracked Moderate Severity
- Contains relevant incidents that have at least 1 impacted site and with moderate severity defined by Everstream (No Minor Incidents to prevent bloat)
- *Filters:*
	- Facility Impact Count >= 1
	- Severity: Moderate
	- Tracked = True

**4. Untracked Moderate Severity
-  Contains incidents that we have looked at with the same as the tracked moderate severity version but have actively untracked/de-prioritized
- *Filters:*
	- Facility Impact Count >= 1
	- Severity: Moderate
	- Tracked = True

**5. All Incidents
- Unfiltered version containing all incidents tracked/untracked.
- Default is showing only **Active** events, but we can toggle that in the **"Settings"** modal


## Related Tags:
#Everstream-views #Everstream-configuration