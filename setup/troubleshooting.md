# Elastic SIEM Troubleshooting Guide

## Overview

This document contains common issues encountered during the deployment and operation of the Home SOC Lab Elastic SIEM environment.

The purpose of this guide is to document:
- setup issues
- telemetry problems
- Kibana access problems
- Elastic Agent issues
- log ingestion troubleshooting
- troubleshooting methodology

Troubleshooting is an important SOC skill because analysts and engineers frequently encounter:
- broken telemetry
- missing logs
- unhealthy agents
- ingestion failures
- alerting problems

---

# Environment Overview

| Component | Purpose |
|---|---|
| Elastic Cloud | Hosted SIEM platform |
| Elasticsearch | Log storage engine |
| Kibana | Security investigation interface |
| Elastic Agent | Endpoint telemetry collection |
| Windows Endpoint | Log source |

---

# Issue 1 — Kibana Not Loading

## Problem

Kibana dashboard failed to open after deployment creation.

---

## Symptoms

- browser loading continuously
- timeout errors
- blank Kibana page

---

## Possible Causes

- deployment still initializing
- temporary cloud delay
- browser caching issue
- internet connectivity issue

---

## Troubleshooting Steps

1. Verified Elastic Cloud deployment status
2. Waited several minutes for initialization
3. Refreshed browser
4. Opened Kibana in another browser tab
5. Verified internet connectivity

---

## Resolution

Deployment completed successfully after several minutes and Kibana became accessible.

---

# Issue 2 — Elastic Agent Not Appearing in Fleet

## Problem

Elastic Agent was installed but did not appear in Fleet.

---

## Symptoms

- no agent visible in Fleet
- enrollment delay
- missing endpoint telemetry

---

## Possible Causes

- delayed enrollment
- invalid enrollment token
- internet connectivity issue
- installation interruption

---

## Troubleshooting Steps

1. Verified installation command
2. Confirmed enrollment token validity
3. Checked internet connectivity
4. Waited several minutes
5. Refreshed Fleet dashboard

---

## Resolution

Agent successfully appeared after enrollment synchronization completed.

---

# Issue 3 — Agent Status Unhealthy

## Problem

Elastic Agent status displayed as unhealthy.

---

## Symptoms

```text
Status: Unhealthy
```

---

## Possible Causes

- communication failure
- network issue
- telemetry interruption
- temporary Elastic Cloud delay

---

## Troubleshooting Steps

1. Refreshed Fleet dashboard
2. Restarted Elastic Agent service
3. Verified endpoint internet connectivity
4. Checked Elastic Cloud availability

---

## Resolution

Agent communication restored and status changed to healthy.

---

# Issue 4 — Logs Not Appearing in Discover

## Problem

Telemetry was not visible inside Kibana Discover.

---

## Symptoms

- empty search results
- missing PowerShell logs
- no endpoint activity visible

---

## Possible Causes

- no endpoint activity generated
- ingestion delay
- incorrect time filter
- ECS processing delay

---

## Troubleshooting Steps

1. Generated additional activity:

```powershell
powershell.exe
```

```powershell
cmd.exe
```

```powershell
ping google.com
```

2. Refreshed Discover page
3. Expanded time range
4. Verified Elastic Agent healthy status

---

## Resolution

New telemetry appeared successfully after generating endpoint activity.

---

# Issue 5 — KQL Query Returning No Results

## Problem

KQL query returned empty results.

---

## Example Query

```kql
process.name: "powershell.exe"
```

---

## Possible Causes

- incorrect field name
- no matching telemetry
- time filter issue
- logs not yet indexed

---

## Troubleshooting Steps

1. Verified ECS field name
2. Checked Discover time filter
3. Generated additional PowerShell activity
4. Confirmed telemetry ingestion

---

## Resolution

Correct time range selected and logs became visible.

---

# Issue 6 — No PowerShell Telemetry

## Problem

PowerShell execution was not detected.

---

## Symptoms

- missing PowerShell events
- no process execution telemetry

---

## Possible Causes

- endpoint telemetry delay
- no recent PowerShell activity
- indexing delay

---

## Troubleshooting Steps

1. Opened PowerShell manually
2. Executed commands
3. Refreshed Discover
4. Verified Elastic Agent healthy status

---

## Resolution

PowerShell telemetry appeared successfully after additional activity generation.

---

# Issue 7 — Slow Search Results

## Problem

Kibana searches were delayed.

---

## Possible Causes

- indexing delay
- cloud resource limitations
- large search range

---

## Troubleshooting Steps

1. Reduced time range
2. Simplified KQL query
3. Refreshed Discover
4. Waited for indexing completion

---

## Resolution

Search performance improved after reducing search scope.

---

# Issue 8 — Incorrect ECS Field Usage

## Problem

Queries failed because incorrect field names were used.

---

## Example Incorrect Query

```kql
username: "Administrator"
```

---

## Correct ECS Query

```kql
user.name: "Administrator"
```

---

## Resolution

Used ECS-standardized fields.

---

# Troubleshooting Methodology

During troubleshooting, the following workflow was followed:

```text
Identify Problem
        ↓
Check Symptoms
        ↓
Verify Connectivity
        ↓
Verify Agent Health
        ↓
Generate Telemetry
        ↓
Check Discover
        ↓
Validate ECS Fields
        ↓
Confirm Resolution
```

---

# Important Troubleshooting Areas

SOC analysts must understand how to troubleshoot:
- missing telemetry
- unhealthy agents
- ingestion failures
- ECS field issues
- detection failures
- alert generation problems

Troubleshooting is critical because:
- detections depend on telemetry
- investigations depend on logs
- alerts depend on proper ingestion

---

# Key Learning Outcomes

During troubleshooting, the following concepts were learned:

- telemetry troubleshooting
- Elastic Agent diagnostics
- Fleet monitoring
- ECS validation
- Kibana troubleshooting
- KQL troubleshooting
- ingestion verification
- endpoint telemetry analysis

---

# Important ECS Fields Verified

```text
host.name
user.name
process.name
process.command_line
source.ip
destination.ip
event.action
```

---

# Conclusion

Troubleshooting the Elastic SIEM environment improved understanding of:
- SIEM architecture
- telemetry collection
- data ingestion
- ECS normalization
- Kibana searching
- Elastic Agent operations

Troubleshooting skills are essential for SOC analysts because reliable telemetry is required for:
- threat hunting
- detection engineering
- incident investigation
- alert analysis
- security monitoring
