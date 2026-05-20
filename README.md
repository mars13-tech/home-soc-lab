# Home SOC Lab — Elastic SIEM Project

## Project Overview

This project demonstrates a cloud-based Security Operations Center (SOC) lab built using Elastic SIEM. The lab was created to learn security monitoring, detection engineering, log analysis, and incident investigation using real Windows endpoint telemetry.

The environment uses Elastic Cloud, Elastic Agent, and Kibana to collect, process, store, search, and investigate security events.

---

# Objectives

- Build a working SIEM environment
- Understand log ingestion and data flow
- Learn KQL-based threat hunting
- Create detection rules and alerts
- Investigate suspicious activity
- Simulate real attack scenarios
- Develop SOC analyst skills

---

# Technologies Used

- Elastic Cloud
- Elastic Security
- Elasticsearch
- Kibana
- Elastic Agent
- Windows Event Logs
- KQL (Kibana Query Language)
- EQL (Event Query Language)

---

# SIEM Architecture

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
