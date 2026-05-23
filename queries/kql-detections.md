# KQL Detection Queries

This document contains KQL detection queries created during Day 2 of the Elastic SIEM lab.

The purpose of these detections is to:
- Practice threat hunting
- Understand suspicious behavior
- Build detection engineering skills
- Investigate endpoint telemetry
- Learn SOC analyst workflows

---

# 1. PowerShell Execution

## Query

```kql
process.name: "powershell.exe"
```

## Purpose

Detects PowerShell execution activity on Windows endpoints.

## Why It Matters

PowerShell is commonly abused by attackers for:
- malware execution
- persistence
- downloading payloads
- lateral movement

## Investigation Steps

- Review user.name
- Review host.name
- Check command line arguments
- Verify parent process

---

# 2. CMD Execution

## Query

```kql
process.name: "cmd.exe"
```

## Purpose

Detects command prompt execution activity.

## Why It Matters

Attackers often use CMD for:
- executing scripts
- running payloads
- launching malicious commands

## Investigation Steps

- Review executed commands
- Identify parent process
- Review user activity

---

# 3. Failed Login Attempts

## Query

```kql
event.action: "logon-failed"
```

## Purpose

Detects failed authentication attempts.

## Why It Matters

Multiple failed logins may indicate:
- brute force attacks
- password spraying
- unauthorized access attempts

## Investigation Steps

- Review source.ip
- Count failed attempts
- Identify targeted accounts

---

# 4. Successful Login Activity

## Query

```kql
event.action: "logged-in"
```

## Purpose

Detects successful authentication events.

## Why It Matters

Used to:
- verify account access
- investigate compromised users
- identify unusual login activity

## Investigation Steps

- Review login timing
- Review host.name
- Check source.ip

---

# 5. Encoded PowerShell Execution

## Query

```kql
process.command_line: *EncodedCommand*
```

## Purpose

Detects encoded PowerShell commands.

## Why It Matters

Attackers use encoded commands to:
- bypass detection
- hide malicious payloads
- evade analysts

## Investigation Steps

- Review full command line
- Decode payload if possible
- Identify parent process

---

# 6. Network Connection Activity

## Query

```kql
destination.ip: *
```

## Purpose

Detects outbound network communication.

## Why It Matters

Can identify:
- command and control traffic
- suspicious external communication
- malware callbacks

## Investigation Steps

- Review destination.ip
- Check connection frequency
- Investigate unusual destinations

---

# 7. Administrator Account Usage

## Query

```kql
user.name: "Administrator"
```

## Purpose

Monitors activity involving the Administrator account.

## Why It Matters

Administrative accounts are high-value targets.

Attackers abuse them for:
- privilege escalation
- persistence
- lateral movement

## Investigation Steps

- Review executed processes
- Investigate login sources
- Verify legitimacy

---

# 8. PowerShell Script Execution

## Query

```kql
process.command_line: *.ps1*
```

## Purpose

Detects PowerShell script execution.

## Why It Matters

PowerShell scripts can be used for:
- malware delivery
- persistence
- automation abuse

## Investigation Steps

- Review script path
- Analyze script behavior
- Identify initiating user

---

# 9. Remote Access Tool Detection

## Query

```kql
process.name: "TeamViewer.exe" OR process.name: "AnyDesk.exe"
```

## Purpose

Detects remote access software execution.

## Why It Matters

Attackers may use remote tools for:
- unauthorized remote access
- persistence
- lateral movement

## Investigation Steps

- Verify legitimate usage
- Review user activity
- Investigate remote sessions

---

# 10. WINWORD Spawning PowerShell

## Query

```kql
process.parent.name: "WINWORD.EXE" AND process.name: "powershell.exe"
```

## Purpose

Detects Microsoft Word spawning PowerShell.

## Why It Matters

This behavior is commonly associated with:
- phishing attacks
- malicious macros
- malware execution

## Investigation Steps

- Review opened document
- Analyze PowerShell command
- Investigate related network activity

---

# Key Learning Outcomes

This exercise helped develop:
- KQL query writing
- Threat hunting skills
- Detection engineering mindset
- Endpoint investigation techniques
- ECS field understanding

---

# Important ECS Fields Used

```text
process.name
process.command_line
process.parent.name
user.name
host.name
event.action
source.ip
destination.ip
```

---

# Conclusion

These KQL detections represent foundational SOC analyst detection engineering skills using Elastic SIEM.

The queries were tested inside Kibana Discover using endpoint telemetry collected from a Windows system through Elastic Agent.
