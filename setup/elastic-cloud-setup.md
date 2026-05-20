# Elastic Cloud Setup Guide

## Overview

This document explains how the Elastic Cloud SIEM environment was deployed and configured for the Home SOC Lab project.

The purpose of this setup was to create a fully functional cloud-based SIEM capable of:
- collecting endpoint telemetry
- storing logs
- performing threat hunting
- creating detections
- generating alerts
- supporting investigations

---

# Environment Components

| Component | Purpose |
|---|---|
| Elastic Cloud | Hosted SIEM platform |
| Elasticsearch | Log storage and search engine |
| Kibana | Visualization and investigation platform |
| Elastic Agent | Endpoint telemetry collector |
| Windows Endpoint | Log source |

---

# Prerequisites

Before starting setup:

- Windows system
- Administrator privileges
- Internet connection
- Elastic Cloud account
- Modern web browser

---

# Step 1 — Create Elastic Cloud Account

Navigate to:

https://cloud.elastic.co

Create:
- Elastic Cloud account
- Trial deployment

---

# Step 2 — Create Deployment

After login:

1. Click:
   Create Deployment

2. Select:
   - Latest Elastic version
   - Closest geographic region
   - Default hardware profile

3. Deployment Name:

```text
home-soc-lab
```

4. Click:
   Create Deployment

Deployment creation may take several minutes.

---

# Step 3 — Open Kibana

After deployment finishes:

Click:
```text
Open Kibana
```

Kibana is used for:
- searching logs
- threat hunting
- dashboard creation
- detections
- alerts
- investigations

---

# Step 4 — Explore Security Section

Inside Kibana:

Navigate to:

```text
Security
```

Sections explored:
- Overview
- Alerts
- Hosts
- Timelines
- Dashboards
- Discover

---

# Step 5 — Configure Fleet

Fleet is used to manage Elastic Agents.

Navigate to:

```text
Management → Fleet
```

Fleet capabilities:
- agent enrollment
- integration management
- telemetry monitoring
- policy management

---

# Step 6 — Add Elastic Agent

Inside Fleet:

1. Click:
```text
Add Agent
```

2. Select:
```text
Windows
```

3. Copy generated installation command.

Example:

```powershell
elastic-agent install --url=https://xxxx.elastic-cloud.com --enrollment-token=XXXX
```

---

# Step 7 — Install Elastic Agent

Open PowerShell as Administrator.

Run the installation command.

Elastic Agent performs:
- endpoint monitoring
- telemetry collection
- log forwarding
- event ingestion

---

# Step 8 — Verify Agent Health

Navigate to:

```text
Fleet → Agents
```

Verify:
```text
Status = Healthy
```

Healthy status confirms:
- successful enrollment
- active telemetry collection
- connectivity with Elastic Cloud

---

# Step 9 — Generate Test Activity

The following commands were executed to generate logs:

```powershell
powershell.exe
```

```powershell
cmd.exe
```

```powershell
ping google.com
```

These commands generated endpoint telemetry for testing.

---

# Step 10 — Verify Logs in Kibana

Navigate to:

```text
Discover
```

Run query:

```kql
process.name: "powershell.exe"
```

Successful results confirmed:
- log ingestion working
- ECS normalization functioning
- Elasticsearch indexing operational

---

# ECS Verification

Observed ECS fields:

```text
host.name
user.name
process.name
process.command_line
source.ip
destination.ip
event.action
```

This confirmed proper field normalization.

---

# Challenges Encountered

## Issue 1 — Agent Not Appearing

### Cause
Enrollment delay after installation.

### Resolution
Waited several minutes and refreshed Fleet dashboard.

---

## Issue 2 — Logs Not Visible Immediately

### Cause
Telemetry ingestion delay.

### Resolution
Generated additional endpoint activity and refreshed Discover.

---

# Key Learning Outcomes

During setup, the following concepts were learned:

- Elastic Cloud deployment
- Kibana navigation
- Fleet management
- Elastic Agent enrollment
- Endpoint telemetry collection
- ECS normalization
- Log verification
- Basic threat hunting workflow

---

# Final Architecture

```text
Windows Endpoint
        ↓
Elastic Agent
        ↓
Elastic Cloud
        ↓
Elasticsearch
        ↓
Kibana
        ↓
Detection Rules
        ↓
Security Alerts
```

---

# Conclusion

The Elastic Cloud SIEM environment was successfully deployed and validated using Windows endpoint telemetry.

The environment now supports:
- log collection
- KQL searching
- detection engineering
- alert generation
- dashboard creation
- incident investigation
- threat hunting workflows

This setup forms the foundation for the remaining SOC lab exercises and security investigations.
