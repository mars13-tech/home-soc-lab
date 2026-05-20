# Elastic SIEM Data Flow

## Overview

This document explains how security events travel through the Elastic SIEM pipeline from endpoint activity to alert generation.

Understanding the data flow is critical for detection engineering, threat hunting, and incident investigation.

---

# Full Elastic SIEM Data Flow

```text
User Activity
     ↓
Windows Event Log
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
