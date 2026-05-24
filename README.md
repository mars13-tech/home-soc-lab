# Home SOC Lab — Elastic SIEM Project

## Project Overview

This project demonstrates a cloud-based Security Operations Center (SOC) lab built using Elastic SIEM. The lab was created to learn security monitoring, detection engineering, log analysis, and incident investigation using real Windows endpoint telemetry.

The environment uses Elastic Cloud, Elastic Agent, Elasticsearch, and Kibana to collect, process, store, search, and investigate security events.

This project was built completely from scratch as part of a hands-on SOC analyst training roadmap focused on practical blue team skills.

---

# Objectives

The main objectives of this project are:

- Build a working Elastic SIEM environment
- Understand SIEM architecture and data flow
- Learn Elastic Common Schema (ECS)
- Practice threat hunting using KQL
- Create detection rules and alerts
- Investigate suspicious activity
- Simulate attack scenarios
- Develop real SOC analyst skills

---

# Technologies Used

## SIEM Platform
- Elastic Cloud
- Elastic Security
- Elasticsearch
- Kibana
- Elastic Agent

## Operating System
- Windows 11

## Query Languages
- KQL (Kibana Query Language)
- EQL (Event Query Language)

## Log Sources
- Windows Event Logs
- Endpoint telemetry
- Process execution logs
- Authentication logs
- Network activity logs

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
```

---

# Data Flow Explanation

The complete data flow inside the SIEM environment:

1. A user performs an activity on the Windows endpoint
2. Windows generates event logs
3. Elastic Agent collects telemetry from the system
4. Elastic ingest pipelines process the raw events
5. ECS normalizes fields into a standard structure
6. Elasticsearch stores searchable event data
7. Kibana visualizes logs and alerts
8. Detection rules analyze the telemetry
9. Alerts are generated when suspicious activity matches rule logic

---

# Understanding ECS

ECS (Elastic Common Schema) standardizes field names across different log sources.

Example:

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

This normalization makes:
- Threat hunting easier
- Detection rules consistent
- Correlation faster
- Investigations more efficient

---

# Important ECS Fields

| ECS Field | Purpose |
|---|---|
| host.name | Identifies affected system |
| user.name | Identifies user activity |
| process.name | Identifies executed process |
| process.command_line | Shows full command execution |
| source.ip | Shows source IP address |
| destination.ip | Shows destination IP |
| event.action | Describes performed action |
| event.category | Categorizes event type |
| event.outcome | Shows success or failure |

---

# Skills Demonstrated

## SIEM Deployment
- Configured Elastic Cloud deployment
- Connected Windows endpoint telemetry
- Verified agent health and data ingestion

## Threat Hunting
- Searched endpoint telemetry using KQL
- Investigated PowerShell execution activity
- Explored authentication and network events

## Detection Engineering
- Created KQL-based detections
- Developed alert rules
- Tuned alerts to reduce false positives

## Log Analysis
- Analyzed Windows process execution logs
- Investigated authentication telemetry
- Explored ECS normalized fields

## Incident Investigation
- Investigated suspicious process execution
- Analyzed process relationships
- Reviewed endpoint telemetry timelines

---

# Detection Use Cases

This project includes detections for:

- PowerShell execution
- Encoded PowerShell commands
- Failed login attempts
- Brute force activity
- Privilege escalation
- Suspicious process execution
- Network communication activity
- Linux SSH login failures
- Process chain analysis
- Administrative activity monitoring

---

# Example KQL Queries

## PowerShell Execution Detection

```kql
process.name: "powershell.exe"
```

---

## Failed Login Detection

```kql
event.action: "logon-failed"
```

---

## Encoded PowerShell Detection

```kql
process.command_line: *EncodedCommand*
```

---

## Network Activity Detection

```kql
destination.ip: *
```

---

# Dashboards

The project includes dashboards for:

- Login activity monitoring
- Process execution visibility
- Security event monitoring
- Threat hunting workflows
- Endpoint activity analysis

Dashboard screenshots are stored inside the `/dashboards` directory.

---

# Detection Rules

The project includes detection rules for:

- PowerShell abuse
- Brute force login attempts
- Privilege escalation activity
- Suspicious command execution
- Authentication anomalies

Detection documentation is stored inside the `/detection-rules` directory.

---

# Investigation Workflow

The investigation process followed in this lab includes:

1. Alert review
2. Event validation
3. Process analysis
4. User activity review
5. Timeline investigation
6. Network analysis
7. Incident conclusion
8. Response recommendation

---

# Simulated Attack Investigation

A simulated PowerShell attack chain was investigated during this project.

Attack Flow:

```text
WINWORD.EXE
        ↓
powershell.exe
        ↓
Network Connection
```

Investigation details are documented inside the `/investigations` directory.

---

# Repository Structure

```text
home-soc-lab/
│
├── README.md
│
├── architecture/
│ ├── data-flow.md
│ └── siem-architecture.png
│
├── setup/
│ ├── elastic-cloud-setup.md
│ ├── agent-install.md
│ └── troubleshooting.md
│
├── dashboards/
│ ├── discover.png
│ ├── powershell-logs.png
│ └── fleet-agent.png
│
├── notes/
│ ├── ecs-fields.md
│ └── discover-notes.md
│
├── queries/
│
├── detection-rules/
│
├── investigations/
│
├── alerts/
│
└── attack-simulation/
```

---

# Key Learning Outcomes

This project helped develop understanding of:

- SIEM architecture
- Elastic data flow
- ECS normalization
- KQL threat hunting
- Detection engineering
- Log analysis
- Alert creation
- Security investigations
- Endpoint telemetry analysis

---

# Future Improvements

Future enhancements planned for this lab:

- Sysmon integration
- MITRE ATT&CK mapping
- Sigma rule conversion
- Advanced EQL detections
- Threat intelligence integration
- Linux endpoint telemetry
- Automated response workflows

---

# Personal Reflection

This project provided hands-on experience with real SIEM workflows used in Security Operations Centers.

Instead of only studying theory, the lab focused on:
- Building the environment
- Understanding telemetry
- Investigating activity
- Creating detections
- Thinking like a SOC analyst

This project significantly improved practical understanding of blue team operations and detection engineering fundamentals.

---

## Author

**Karthikeyan**  
Cybersecurity Engineering Student | Blue Team | SOC Analyst in the Making  
🔗 [LinkedIn](https://www.linkedin.com/in/karthi-keyan-9042862bb)  
🐙 [GitHub](https://github.com/mars13-tech)
