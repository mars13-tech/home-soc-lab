# Phishing Investigation

## Alert Summary

An alert triggered for Microsoft Word spawning PowerShell activity.

---

# Detection Query

```kql
process.parent.name: "WINWORD.EXE"
AND process.name: "powershell.exe"
```

---

# Investigation Timeline

1. Microsoft Word process executed
2. WINWORD.EXE launched PowerShell
3. PowerShell command execution detected
4. Encoded command analysis performed
5. Related network activity reviewed

---

# Investigation Findings

The investigation identified suspicious parent-child process behavior.

Observed behavior:
WINWORD.EXE
→ powershell.exe

This activity may indicate:
- malicious Office macro execution
- phishing document activity
- malware delivery attempts

---

# Affected Assets

## User

[user.name]

## Host

[host.name]

---

# Severity Assessment

High

Reason:
Office applications normally should not execute PowerShell processes.

---

# MITRE ATT&CK Mapping

## Tactic

Execution

## Technique

PowerShell

---

# Conclusion

The detection successfully identified suspicious Office-to-PowerShell execution behavior.

The investigation demonstrated:
- process analysis
- event correlation
- timeline reconstruction
- SOC investigation workflow
