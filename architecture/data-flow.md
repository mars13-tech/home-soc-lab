# Elastic SIEM Data Flow

## Overview

This document explains how security events travel through the Elastic SIEM pipeline from endpoint activity to alert generation.

Understanding SIEM data flow is critical for:
- threat hunting
- detection engineering
- incident investigation
- alert analysis
- SOC operations

This lab uses Elastic Cloud with Windows endpoint telemetry.

---

# Full Data Flow Architecture

```text
User Activity
      ↓
Windows Event Logs
      ↓
Elastic Agent
      ↓
Elastic Ingest Pipeline
      ↓
ECS Normalization
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

# Step-by-Step Data Flow

---

# Step 1 — User Activity

Everything begins with activity on the endpoint system.

Examples of user activity:
- user login
- PowerShell execution
- CMD execution
- file access
- network connection
- scheduled task creation

Example:

```powershell
powershell.exe
```

When this command executes, Windows automatically creates telemetry related to the process activity.

---

# Step 2 — Windows Event Logs

Windows records operating system activity inside Event Logs.

Important log categories:
- Security logs
- System logs
- Application logs
- PowerShell logs

These logs contain important information such as:
- username
- process name
- timestamps
- source IP
- destination IP
- command line
- host information

Example raw event:

```text
New Process Name: powershell.exe
Creator Process Name: cmd.exe
User: Administrator
```

Raw logs are difficult to analyze consistently because different systems and products use different field names and formats.

---

# Step 3 — Elastic Agent

Elastic Agent is installed on the Windows endpoint.

Role of Elastic Agent:
- collect telemetry
- monitor endpoint activity
- forward logs to Elastic Cloud
- manage integrations
- support endpoint visibility

Elastic Agent acts as the telemetry collector for the SIEM platform.

Without Elastic Agent:
- no visibility
- no log ingestion
- no detections
- no alerts

---

# Step 4 — Elastic Ingest Pipeline

After collection, Elastic processes incoming events using ingest pipelines.

The ingest pipeline performs:
- parsing
- field extraction
- data formatting
- event preparation
- normalization support

Example transformation:

Raw Log:

```text
New Process Name: powershell.exe
```

Processed Event:

```text
process.name: powershell.exe
```

This makes security data easier to search and investigate.

---

# Step 5 — ECS Normalization

ECS stands for Elastic Common Schema.

ECS standardizes field names across different log sources.

Without ECS:

```text
username
user
account
login_user
```

With ECS:

```text
user.name
```

This standardization allows analysts to:
- write reusable detections
- perform consistent threat hunting
- correlate events across products
- simplify investigations

Common ECS fields:

```text
host.name
user.name
process.name
process.command_line
source.ip
destination.ip
event.action
```

ECS is critical for:
- SIEM scalability
- detection engineering
- cross-source correlation
- alerting
- investigation workflows

---

# Step 6 — Elasticsearch

Elasticsearch stores normalized logs as searchable documents.

Role of Elasticsearch:
- data storage
- indexing
- fast searching
- powering detections
- supporting dashboards

Think of Elasticsearch as the searchable database engine of the SIEM.

Example query:

```kql
process.name: "powershell.exe"
```

Elasticsearch rapidly returns matching events.

---

# Step 7 — Kibana

Kibana is the user interface for Elastic SIEM.

Security analysts use Kibana to:
- search logs
- investigate alerts
- perform threat hunting
- create dashboards
- build detections
- analyze timelines

Important Kibana sections:
- Discover
- Security
- Alerts
- Dashboards
- Timelines
- Fleet

---

# Step 8 — Detection Rules

Detection rules continuously analyze incoming telemetry.

Rules are built using:
- KQL
- EQL
- threshold logic

Example detection rule:

```kql
process.name: "powershell.exe" AND process.command_line: *EncodedCommand*
```

This rule detects suspicious encoded PowerShell execution.

If activity matches detection logic:
- Elastic generates an alert

---

# Step 9 — Security Alerts

Alerts are generated when suspicious activity matches detection rules.

Alerts contain:
- hostname
- username
- process details
- timestamps
- severity
- investigation metadata

Security analysts investigate alerts to determine:
- true positive
- false positive
- malicious activity
- attack severity

---

# Example — PowerShell Alert Flow

---

## Step 1 — User Executes PowerShell

Example:

```powershell
powershell.exe -EncodedCommand XXXXX
```

---

## Step 2 — Windows Creates Event

Windows records process creation activity inside Event Logs.

---

## Step 3 — Elastic Agent Collects Event

Elastic Agent forwards telemetry to Elastic Cloud.

---

## Step 4 — Ingest Pipeline Parses Event

Raw log data is processed and fields are extracted.

---

## Step 5 — ECS Standardizes Fields

Example normalized fields:

```text
process.name: powershell.exe
process.command_line: powershell.exe -EncodedCommand XXXXX
```

---

## Step 6 — Elasticsearch Stores Event

The event becomes searchable inside Elasticsearch.

---

## Step 7 — Kibana Displays Event

The analyst searches telemetry in Discover.

Example:

```kql
process.name: "powershell.exe"
```

---

## Step 8 — Detection Rule Matches Activity

Detection rule identifies suspicious encoded PowerShell execution.

---

## Step 9 — Alert Generated

Elastic creates a security alert for investigation.

---

# Why Understanding Data Flow Matters

Understanding SIEM data flow allows analysts to:
- troubleshoot ingestion issues
- build accurate detections
- investigate incidents effectively
- understand attacker behavior
- reduce false positives
- improve alert quality

SOC analysts must understand:
- where logs originate
- how logs move
- how logs are normalized
- how detections work
- how alerts are generated

---

# Key Learning Outcomes

During this lab, the following concepts were learned:

- SIEM architecture
- telemetry collection
- log pipelines
- ECS normalization
- Elasticsearch indexing
- Kibana threat hunting
- KQL searching
- alert generation
- incident investigation

---

# Final Architecture Summary

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

Understanding Elastic SIEM data flow is critical for becoming an effective SOC analyst.

A strong understanding of:
- telemetry collection
- ingest pipelines
- ECS normalization
- data indexing
- detection logic
- alert generation

enables analysts to build detections, investigate incidents, and identify malicious activity efficiently inside Elastic SIEM.
