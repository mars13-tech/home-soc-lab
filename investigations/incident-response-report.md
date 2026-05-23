# Incident Response Report

## Incident Summary

A suspicious PowerShell execution event was detected involving Microsoft Word spawning PowerShell.

---

# Detection Details

## Detection Query

```kql
process.parent.name: "WINWORD.EXE"
AND process.name: "powershell.exe"
```

---

# Investigation Timeline

1. User activity observed
2. Microsoft Word process executed
3. WINWORD.EXE launched PowerShell
4. Encoded command execution reviewed
5. Related telemetry analyzed

---

# Affected Assets

## User

[user.name]

## Host

[host.name]

---

# Investigation Findings

The activity demonstrated suspicious Office-to-PowerShell execution behavior.

Possible indicators:
- phishing activity
- malicious macro execution
- attacker payload delivery

---

# Severity Assessment

High

Reason:
Office applications normally should not execute PowerShell commands.

---

# Response Actions

- investigated alert
- reviewed telemetry
- analyzed process chain
- reviewed command execution
- assessed potential compromise

---

# Containment Recommendations

- isolate affected endpoint
- review user activity
- reset credentials if necessary
- scan system for malware

---

# Lessons Learned

The incident demonstrated the importance of:
- behavioral detections
- process chain analysis
- alert triage
- investigation workflows

---

# Conclusion

The Elastic SIEM successfully detected and supported investigation of suspicious PowerShell execution activity.
