# Elastic Agent Installation Guide

## Overview

This document explains how Elastic Agent was installed and configured on a Windows endpoint for the Home SOC Lab project.

Elastic Agent is responsible for:
- collecting endpoint telemetry
- monitoring system activity
- forwarding logs to Elastic Cloud
- enabling SIEM visibility
- supporting detections and investigations

Without Elastic Agent:
- logs are not collected
- telemetry is unavailable
- detections cannot function
- alerts cannot be generated

---

# Environment Details

| Component | Purpose |
|---|---|
| Windows Endpoint | Log source |
| Elastic Agent | Telemetry collector |
| Elastic Cloud | SIEM platform |
| Kibana | Security monitoring interface |

---

# Prerequisites

Before installation:

- Elastic Cloud deployment active
- Kibana accessible
- Fleet enabled
- Administrator privileges on Windows
- Internet connection

---

# Step 1 — Open Fleet

Inside Kibana:

Navigate to:

```text
Management → Fleet
```

Fleet is used for:
- managing agents
- enrolling endpoints
- monitoring telemetry
- handling integrations

---

# Step 2 — Add New Agent

Inside Fleet:

Click:

```text
Add Agent
```

---

# Step 3 — Select Windows

Choose:

```text
Windows
```

This generates an installation command containing:
- Elastic Cloud URL
- enrollment token

---

# Step 4 — Copy Installation Command

Example command:

```powershell
elastic-agent install --url=https://xxxx.elastic-cloud.com --enrollment-token=XXXX
```

The enrollment token securely connects the endpoint to Elastic Cloud.

---

# Step 5 — Open PowerShell as Administrator

Elastic Agent installation requires administrative privileges.

Steps:
1. Open Start Menu
2. Search:
   ```text
   PowerShell
   ```
3. Right-click:
   ```text
   Run as Administrator
   ```

---

# Step 6 — Run Installation Command

Paste the copied installation command into PowerShell.

Example:

```powershell
elastic-agent install --url=https://xxxx.elastic-cloud.com --enrollment-token=XXXX
```

The installation process:
- installs Elastic Agent
- registers the endpoint
- connects to Elastic Cloud
- starts telemetry collection

---

# Step 7 — Wait for Installation Completion

Successful installation output confirms:
- service installation
- enrollment success
- agent startup

Elastic Agent now runs as a Windows service.

---

# Step 8 — Verify Agent Health

Inside Kibana:

Navigate to:

```text
Fleet → Agents
```

Expected status:

```text
Healthy
```

Healthy status confirms:
- successful enrollment
- active connectivity
- telemetry collection functioning

---

# Step 9 — Generate Endpoint Activity

To verify telemetry ingestion, several commands were executed.

Examples:

```powershell
powershell.exe
```

```powershell
cmd.exe
```

```powershell
ping google.com
```

These actions generate endpoint telemetry.

---

# Step 10 — Verify Logs in Kibana

Navigate to:

```text
Discover
```

Run KQL query:

```kql
process.name: "powershell.exe"
```

Successful results confirm:
- log collection functioning
- ECS normalization operational
- telemetry ingestion successful

---

# ECS Fields Observed

The following ECS fields were verified:

```text
host.name
user.name
process.name
process.command_line
source.ip
destination.ip
event.action
```

This confirmed proper event normalization.

---

# How Elastic Agent Works

Elastic Agent continuously monitors endpoint activity and forwards telemetry to Elastic Cloud.

Workflow:

```text
Windows Activity
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
Alerts
```

Elastic Agent acts as the telemetry collection layer of the SIEM.

---

# Example Telemetry Collection

## User Executes PowerShell

Example:

```powershell
powershell.exe
```

---

## Windows Generates Event

Windows records process execution activity.

---

## Elastic Agent Collects Event

Elastic Agent captures the telemetry.

---

## Event Sent to Elastic Cloud

Telemetry is forwarded securely.

---

## ECS Normalization

Fields become standardized:

```text
process.name: powershell.exe
```

---

## Elasticsearch Stores Event

The event becomes searchable.

---

## Kibana Displays Event

Security analysts can investigate the activity.

---

# Common Troubleshooting

---

# Issue 1 — Agent Not Appearing in Fleet

## Cause
Enrollment delay or connectivity issue.

## Resolution
- wait several minutes
- refresh Fleet page
- verify internet connectivity

---

# Issue 2 — Agent Status Unhealthy

## Cause
Telemetry communication problem.

## Resolution
- restart Elastic Agent service
- verify Elastic Cloud availability
- check firewall connectivity

---

# Issue 3 — Logs Not Visible

## Cause
No endpoint activity generated.

## Resolution
Generate additional PowerShell or CMD activity and refresh Discover.

---

# Security Benefits of Elastic Agent

Elastic Agent provides:
- endpoint visibility
- centralized telemetry collection
- process monitoring
- network monitoring
- detection support
- investigation support

It forms the foundation of endpoint telemetry inside Elastic SIEM.

---

# Key Learning Outcomes

During installation, the following concepts were learned:

- Elastic Agent enrollment
- Fleet management
- endpoint telemetry collection
- ECS normalization
- log verification
- telemetry troubleshooting
- SIEM data flow
- Kibana searching

---

# Conclusion

Elastic Agent was successfully installed and connected to Elastic Cloud.

The endpoint now provides:
- real-time telemetry
- process visibility
- searchable logs
- detection support
- investigation data

This configuration forms the telemetry foundation for threat hunting, detection engineering, and incident investigation inside the Home SOC Lab.
