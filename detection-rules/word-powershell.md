# Microsoft Word Spawning PowerShell Detection

## Detection Objective

Detect Microsoft Word launching PowerShell processes.

---

# KQL Query

```kql
process.parent.name: "WINWORD.EXE"
AND process.name: "powershell.exe"
```

---

# Severity

High

---

# MITRE ATT&CK Mapping

## Tactic

Execution

## Technique

PowerShell

---

# Why This Detection Matters

Microsoft Word normally should not launch PowerShell.

This behavior commonly indicates:
- phishing attacks
- malicious macros
- malware execution
- attacker payload delivery

---

# Investigation Guide

Investigate:
- opened Office document
- PowerShell command line
- user activity
- network connections
- downloaded files
- child processes

---

# Parent-Child Process Analysis

Parent-child analysis is important because it helps analysts understand process relationships and attacker behavior.

Example:

WINWORD.EXE
→ powershell.exe

This chain may indicate malicious document execution.

---

# Potential False Positives

Possible legitimate causes:
- administrative automation
- trusted enterprise macros
- approved business scripts

---

# Key Lessons

Behavior-based detections are stronger than simple keyword detections because they focus on suspicious activity chains.
