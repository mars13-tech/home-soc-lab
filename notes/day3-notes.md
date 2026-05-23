# Day 3 Notes — Detection Engineering and Alerting

## Topics Learned

- Detection engineering fundamentals
- Elasticsearch query rules
- Threshold rules
- Alert validation
- Alert tuning
- SOC alert triage
- False positives
- Parent-child process analysis

---

# Important Concepts

## Detection Engineering

Detection engineering is the process of converting attacker behavior into automated detection logic.

---

## Alert Tuning

Alert tuning improves detection quality by reducing false positives and improving investigation accuracy.

---

## False Positives

A false positive occurs when legitimate activity is incorrectly identified as malicious.

---

## Alert Fatigue

Too many alerts may overwhelm analysts and cause real attacks to be missed.

---

# High-Value Detection Examples

## Encoded PowerShell

```kql
process.name: "powershell.exe"
AND process.command_line: *EncodedCommand*
```

---

## Word Spawning PowerShell

```kql
process.parent.name: "WINWORD.EXE"
AND process.name: "powershell.exe"
```

---

# Key Lessons

Strong detections:
- focus on attacker behavior
- reduce noise
- provide investigation context
- improve SOC efficiency
