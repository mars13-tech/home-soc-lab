# Notes Index — Home SOC Lab

**Author:** Karthikeyan  
**Lab:** Home SOC Lab — Elastic SIEM  
**Purpose:** Quick reference index for all learning notes in this folder  

---

## What This Folder Contains

Day-by-day learning notes documenting real progress building and operating
a home SOC lab on Elastic SIEM. Each file covers what was built, what broke,
what was fixed, and what was learned.

Reading these in order shows the full progression from zero to working
detection engineering and incident investigation.

---

## Notes in Reading Order

### Day 2 — [day2-learning-notes.md](./day2-learning-notes.md)
**Covers:**
- Elastic Cloud deployment from scratch
- First agent connection to Fleet
- Initial telemetry appearing in Kibana Discover
- First look at raw Windows endpoint logs

**Key learning:** How Elastic Agent communicates with Elasticsearch
and why the index pattern matters for querying data.

---

### Day 3 — [day3-notes.md](./day3-notes.md)
**Covers:**
- ECS (Elastic Common Schema) field structure
- Navigating Kibana Discover effectively
- Understanding how fields are normalised across log sources
- First KQL filter queries

**Key learning:** ECS is what makes threat hunting consistent.
Without it, every log source uses different field names and
correlation becomes impossible.

---

### Day 4 — [day4-notes.md](./day4-notes.md)
**Covers:**
- Writing first real KQL queries
- PowerShell log analysis — finding process.name and process.command_line
- Filtering by user.name and host.name
- Understanding event.action and event.category fields

**Key learning:** KQL is simple but powerful. The real skill is
knowing which ECS fields to filter on for each attack technique.

---

### Day 5 — [day5-notes.md](./day5-notes.md)
**Covers:**
- Creating first detection rule in Elastic Security
- Configuring alert conditions and severity
- Understanding rule schedule and look-back window
- First alert firing successfully

**Key learning:** Writing the rule is 20% of the work.
Understanding the schedule, look-back, and threshold settings
is what makes a rule actually reliable in production.

---

### Day 6 — [day6-notes.md](./day6-notes.md)
**Covers:**
- Alert tuning — reducing false positives
- Whitelisting legitimate activity without breaking detection
- Understanding the difference between noise and signal
- Alert workflow from fire to close

**Key learning:** Alert tuning is harder than writing the
detection rule. A rule that fires on everything is as useless
as no rule at all. Tuning is where real SOC skill lives.

---

## Reference Notes

### [ecs-fields.md](./ecs-fields.md)
**What it is:** ECS field reference for SOC analyst use  
**When to use:** When writing KQL queries or detection rules —
look up the correct field name here before querying

**Key fields covered:**

| Field | Purpose |
|---|---|
| `host.name` | Identifies affected system |
| `user.name` | Identifies user activity |
| `process.name` | Identifies executed process |
| `process.command_line` | Shows full command execution |
| `process.parent.name` | Shows parent process |
| `event.action` | Describes performed action |
| `event.category` | Categorizes event type |
| `event.outcome` | Shows success or failure |
| `source.ip` | Shows source IP address |
| `destination.ip` | Shows destination IP |

---

### [discover-notes.md](./discover-notes.md)
**What it is:** Kibana Discover workflow reference  
**When to use:** When starting a threat hunting session or
investigation — reference for navigating Discover efficiently

**Key workflows covered:**
- Setting index pattern and time range
- Adding and removing field columns
- Saving searches for reuse
- Pivoting from one field to related events

---

## Key Lessons Across All Notes

| Lesson | Where It Came From |
|---|---|
| Debugging log ingestion taught more than any tutorial | Day 2 troubleshooting |
| ECS field names are the foundation of all KQL queries | Day 3 |
| PowerShell process.command_line is the most important field for detecting abuse | Day 4 |
| Detection rule look-back window directly affects alert reliability | Day 5 |
| Alert tuning is harder than detection rule writing | Day 6 |

---

## Connected Files in This Lab

- `detection-rules/` — detection rules built using knowledge from day 4 and 5
- `queries/kql-threat-hunting.md` — full query library built from day 4 notes
- `alerts/alert-tuning.md` — detailed tuning documentation from day 6
- `investigations/` — investigation workflows built on day 5 and 6 foundations
