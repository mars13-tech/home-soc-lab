# Encoded PowerShell Detection

## Detection Objective

Detect suspicious encoded PowerShell execution activity.

Encoded PowerShell commands are commonly used by attackers to:
- obfuscate malicious commands
- bypass basic monitoring
- evade security controls
- execute payloads

---

# Detection Rule

## Rule Type

Elasticsearch Query Rule

---

## KQL Query

```kql
process.name: "powershell.exe"
AND process.command_line: *EncodedCommand*
```

---

# Severity

High

---

# Risk Score

75

---

# MITRE ATT&CK Mapping

## Tactic

Execution

## Technique

PowerShell

---

# Investigation Guide

SOC analysts should investigate:

- user.name
- host.name
- process.command_line
- process.parent.name
- related network activity
- child processes

---

# Why This Detection Matters

Encoded PowerShell is frequently used in:
- phishing attacks
- malware execution
- attacker persistence
- lateral movement
- post-exploitation activity

This behavior is considered high-risk because legitimate users rarely execute encoded PowerShell commands.

---

# Potential False Positives

Possible legitimate cases:
- IT administration scripts
- automation tooling
- enterprise management software

Analysts should validate activity context before escalation.

---

# Validation Process

The detection was validated by executing:

```powershell
powershell -EncodedCommand ZQBjAGgAbwAgAHQAZQBzAHQA
```

The alert successfully triggered in Elastic SIEM.

---

# Key Lessons

Good detections focus on:
- suspicious behavior
- reduced false positives
- investigation context
- attacker techniques
