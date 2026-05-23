# Alert Tuning Notes

## What is Alert Tuning?

Alert tuning is the process of improving detection quality by reducing false positives and increasing investigation accuracy.

---

# Why Alert Tuning Matters

Poorly tuned detections generate excessive alerts and create alert fatigue.

Too many false positives may cause:
- analyst burnout
- missed attacks
- delayed investigations
- reduced SOC efficiency

---

# Weak Detection Example

```kql
process.name: "powershell.exe"
```

Problem:
- excessive alerts
- normal admin activity
- high false positives

---

# Improved Detection

```kql
process.name: "powershell.exe"
AND process.command_line: *EncodedCommand*
```

Improvement:
- more suspicious behavior
- lower false positives
- higher confidence detection

---

# High-Confidence Detection

```kql
process.parent.name: "WINWORD.EXE"
AND process.command_line: *EncodedCommand*
AND NOT user.name: "Administrator"
```

Why High Confidence:
- Office applications normally should not launch encoded PowerShell
- common phishing behavior
- possible malicious macro execution

---

# Key Lessons

Good detections:
- focus on behavior
- reduce noise
- improve investigation quality
- provide meaningful context
