# Threat Hunting Report

## Objective

Perform proactive threat hunting within Elastic SIEM to identify suspicious attacker behavior.

---

# Hunt Areas

- PowerShell abuse
- encoded commands
- LOLBins
- Office spawning scripts
- failed logins
- network activity

---

# Hunt 1 — Encoded PowerShell

## Query

```kql
process.name: "powershell.exe"
AND process.command_line: *EncodedCommand*
```

## Findings

Encoded PowerShell activity was identified.

Possible risks:
- obfuscation
- malware execution
- attacker payload delivery

---

# Hunt 2 — Office Spawning PowerShell

## Query

```kql
process.parent.name: "WINWORD.EXE"
AND process.name: "powershell.exe"
```

## Findings

Office applications launching PowerShell may indicate phishing or malicious macro execution.

---

# Hunt 3 — Suspicious LOLBins

## Query

```kql
process.name: "certutil.exe"
OR process.name: "mshta.exe"
OR process.name: "rundll32.exe"
```

## Findings

LOLBins are legitimate Windows binaries commonly abused by attackers.

---

# Hunt 4 — Failed Login Activity

## Query

```kql
event.action: "logon-failed"
```

## Findings

Repeated failed login attempts may indicate brute force or password spraying activity.

---

# Investigation Lessons

Threat hunting requires:
- proactive analysis
- behavioral understanding
- event correlation
- contextual investigation

---

# Conclusion

The threat hunting exercise demonstrated:
- advanced telemetry analysis
- behavioral hunting
- attacker-focused detection thinking
- Elastic SIEM investigation workflows
