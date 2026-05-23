# Encoded PowerShell Investigation

## Alert Summary

An alert triggered for encoded PowerShell execution activity on a Windows endpoint.

---

# Detection Query

```kql
process.name: "powershell.exe"
AND process.command_line: *EncodedCommand*
```

---

# Investigation Steps

- Reviewed user.name
- Reviewed host.name
- Checked process.command_line
- Verified parent process
- Investigated related activity

---

# Findings

Encoded PowerShell command execution was detected.

The command line contained:
-EncodedCommand

This behavior may indicate:
- obfuscation
- malicious execution
- attacker activity

---

# Severity Assessment

High

Reason:
Encoded PowerShell is commonly used by attackers to hide malicious commands and evade detection.

---

# Conclusion

The activity was successfully detected using Elastic SIEM detection rules.

The alert demonstrated successful:
- telemetry collection
- ECS normalization
- detection engineering
- alert generation
- SOC investigation workflow

---

# Key Lessons

Good SOC investigations focus on:
- behavior analysis
- process relationships
- context correlation
- investigation accuracy
