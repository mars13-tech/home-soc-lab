# Day 2 Learning Notes — KQL and Threat Hunting

## Objective

The goal of Day 2 was to:
- Learn KQL fundamentals
- Understand threat hunting
- Build detection engineering skills
- Investigate endpoint telemetry
- Create practical detection queries

---

# What is KQL?

KQL (Kibana Query Language) is used inside Kibana to:
- search logs
- filter telemetry
- investigate events
- perform threat hunting
- create detection rules

SOC analysts use KQL daily during:
- investigations
- alert triage
- detection engineering
- hunting operations

---

# KQL Mindset

Important realization:

KQL is not just searching logs.

KQL allows analysts to ask questions to telemetry data.

Example:

Question:
"Did PowerShell execute?"

KQL:
```kql
process.name: "powershell.exe"
```

---

# ECS Fields Learned

## process.name

Used to identify executed processes.

Example:
```kql
process.name: "powershell.exe"
```

---

## process.command_line

Shows full command execution details.

Example:
```kql
process.command_line: *EncodedCommand*
```

---

## process.parent.name

Shows parent-child process relationships.

Example:
```kql
process.parent.name: "WINWORD.EXE"
```

---

## user.name

Identifies the user responsible for activity.

---

## host.name

Identifies the endpoint generating logs.

---

## destination.ip

Shows outbound network communication targets.

---

# KQL Operators Learned

## Exact Match

```kql
process.name: "powershell.exe"
```

Used to find exact values.

---

## Wildcards

```kql
process.command_line: *EncodedCommand*
```

Used to search for partial values.

---

## AND Operator

```kql
process.name: "powershell.exe" AND user.name: "Administrator"
```

Used to combine conditions.

---

## OR Operator

```kql
process.name: "cmd.exe" OR process.name: "powershell.exe"
```

Used to match multiple possibilities.

---

## NOT Operator

```kql
NOT process.name: "chrome.exe"
```

Used to exclude activity.

---

# Important Threat Hunting Concepts

## 1. Encoded PowerShell

Attackers often encode PowerShell commands to:
- hide malicious activity
- bypass security monitoring
- evade analysts

---

## 2. Parent-Child Process Relationships

Example:
```text
WINWORD.EXE → powershell.exe
```

This is suspicious because Microsoft Word normally should not launch PowerShell.

This behavior may indicate:
- phishing attacks
- malicious macros
- malware execution

---

## 3. Failed Login Attempts

Multiple failed logins may indicate:
- brute force attacks
- password spraying
- unauthorized access attempts

---

## 4. Administrative Account Monitoring

Administrative accounts are high-value targets.

Monitoring admin activity is critical for detecting:
- privilege escalation
- persistence
- lateral movement

---

# Discover Hunting Practice

The following searches were tested inside Kibana Discover:

```kql
process.name: "powershell.exe"
```

```kql
process.name: "cmd.exe"
```

```kql
event.action: "logon-failed"
```

```kql
destination.ip: *
```

```kql
process.command_line: *EncodedCommand*
```

---

# Key Learning Outcomes

By the end of Day 2:

- Learned KQL fundamentals
- Understood ECS-based hunting
- Built 10 KQL detections
- Practiced endpoint telemetry analysis
- Improved investigation mindset
- Learned suspicious behavior indicators

---

# Personal Reflection

Day 2 significantly improved understanding of:
- detection engineering
- threat hunting
- suspicious process behavior
- telemetry analysis
- SOC analyst workflows

The most important lesson was learning how to think like a defender instead of simply searching logs.
