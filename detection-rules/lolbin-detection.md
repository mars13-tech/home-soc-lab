# Detection Rule — LOLBin Abuse

**Rule Name:** Suspicious LOLBin Execution  
**Severity:** High  
**Platform:** Elastic SIEM / KQL  
**Author:** Karthikeyan  
**Last Updated:** May 2026  
**MITRE ATT&CK:** T1218 — Signed Binary Proxy Execution  

---

## What Are LOLBins?

LOLBins (Living Off the Land Binaries) are legitimate Windows binaries
that attackers abuse to execute malicious code, bypass security controls,
and blend in with normal system activity.

They are dangerous because:

- They are signed by Microsoft — most security tools trust them
- They are already present on every Windows system — no download needed
- They blend into normal activity — hard to detect without context
- They bypass application whitelisting — allowed by default

---

## LOLBins Covered by This Rule

| Binary | Legitimate Use | Attacker Abuse |
|---|---|---|
| `certutil.exe` | Certificate management | Download files, decode Base64 payloads |
| `rundll32.exe` | Load DLL files | Execute malicious DLLs, bypass controls |
| `mshta.exe` | Run HTA applications | Execute remote scripts, VBScript payloads |
| `wscript.exe` | Run Windows scripts | Execute malicious JS or VBS files |
| `cscript.exe` | Run Windows scripts (console) | Execute malicious scripts silently |
| `regsvr32.exe` | Register COM objects | Load remote scriptlets (Squiblydoo) |
| `msiexec.exe` | Install MSI packages | Install malicious packages, load DLLs |
| `bitsadmin.exe` | Background file transfer | Download malicious payloads |

---

## Detection Logic

### Primary KQL Detection — Any LOLBin Execution

```kql
process.name: ("certutil.exe" or "rundll32.exe" or "mshta.exe" or "wscript.exe" or "cscript.exe" or "regsvr32.exe" or "msiexec.exe" or "bitsadmin.exe")
```

> **Use:** Baseline — find all LOLBin activity on endpoint  
> **Next step:** Review parent process and command line arguments  

---

### High Confidence — LOLBin Spawned by Office Application

```kql
process.parent.name: ("winword.exe" or "excel.exe" or "powerpnt.exe" or "outlook.exe") and process.name: ("certutil.exe" or "rundll32.exe" or "mshta.exe" or "wscript.exe" or "cscript.exe" or "regsvr32.exe")
```

> **Use:** Office document spawning LOLBin — almost always malicious  
> **MITRE:** T1566.001, T1218  
> **Next step:** Identify the document, isolate endpoint immediately  

---

### Certutil Used as Downloader

```kql
process.name: "certutil.exe" and process.command_line: (*urlcache* or *split* or *http*)
```

> **Use:** Detect certutil abused to download files from internet  
> **MITRE:** T1218.001, T1105  
> **Next step:** Extract URL from command line, check threat intel feeds  

---

### Regsvr32 Squiblydoo Attack

```kql
process.name: "regsvr32.exe" and process.command_line: (*scrobj* or *http* or */s* or */u*)
```

> **Use:** Detect Squiblydoo — regsvr32 loading remote COM scriptlet  
> **MITRE:** T1218.010  
> **Next step:** Check URL in command line, block and isolate if external  

---

### Mshta Loading Remote Script

```kql
process.name: "mshta.exe" and process.command_line: (*http* or *vbscript* or *javascript*)
```

> **Use:** Detect mshta executing remote or inline scripts  
> **MITRE:** T1218.005  
> **Next step:** Decode and review the script content  

---

### BITSAdmin Download

```kql
process.name: "bitsadmin.exe" and process.command_line: (*transfer* or *download* or */addfile*)
```

> **Use:** Detect BITSAdmin used to download malicious payloads  
> **MITRE:** T1197, T1105  
> **Next step:** Check destination URL, review what was downloaded  

---

## Alert Conditions

Trigger alert when:

- Any LOLBin spawned by Office application — **P1 Critical**
- Certutil with urlcache or http argument — **P2 High**
- Regsvr32 with scrobj or remote URL — **P2 High**
- Mshta with http or script argument — **P2 High**
- Any LOLBin with unusual parent process — **P2 High**
- Standalone LOLBin execution — **P3 Medium** (investigate for context)

---

## Investigation Steps

**Step 1 — Check parent process**
```
What spawned the LOLBin?
- Office app → high confidence malicious
- CMD or PowerShell → investigate further
- Explorer.exe → user likely ran it manually, lower risk
```

**Step 2 — Review command line arguments**
```
Look for:
- URLs (http:// or https://)
- Base64 encoded strings
- File paths pointing to temp directories
- /s /u /i flags on regsvr32
- urlcache or split on certutil
```

**Step 3 — Check network connections**
```
Did the LOLBin make an outbound connection?
- Certutil downloading → check destination IP
- Mshta connecting → extract and analyse script
- BITSAdmin → check transfer destination
```

**Step 4 — Review what was created**
```
Check for new files created after LOLBin execution:
- Temp directories: C:\Windows\Temp, C:\Users\*\AppData\Local\Temp
- Suspicious extensions: .ps1, .vbs, .js, .hta, .dll
```

**Step 5 — Escalation decision**
```
Escalate to L2 if:
- LOLBin spawned by Office application
- Outbound network connection confirmed
- New executable or script created in temp directory
- Command line contains encoded or obfuscated content
```

---

## False Positive Scenarios

| Scenario | How to Identify |
|---|---|
| IT admin using certutil for cert management | Parent process is legitimate admin tool, no http argument |
| Software deployment via msiexec | Known software hash, signed installer |
| Scheduled tasks using cscript | Known script path, consistent schedule |
| Developer using rundll32 for testing | Developer machine, known DLL path |

**Tuning tip:** Whitelist known admin scripts by full path and hash. Never whitelist by binary name alone.

---

## MITRE ATT&CK Full Mapping

| Technique | ID | Sub-technique | Binary |
|---|---|---|---|
| Signed Binary Proxy Execution | T1218 | — | All LOLBins |
| Certutil | T1218 | T1218.001 | certutil.exe |
| Mshta | T1218 | T1218.005 | mshta.exe |
| Regsvr32 | T1218 | T1218.010 | regsvr32.exe |
| Rundll32 | T1218 | T1218.011 | rundll32.exe |
| BITS Jobs | T1197 | — | bitsadmin.exe |
| Ingress Tool Transfer | T1105 | — | certutil, bitsadmin |
| Phishing Attachment | T1566 | T1566.001 | Office → LOLBin chain |

---

## Connected Files in This Lab

- `queries/kql-threat-hunting.md` — LOLBin hunting queries section
- `dashboards/lolbin-hunt.png` — LOLBin threat hunting session screenshot
- `dashboards/process-chain.png` — Parent-child process chain screenshot
- `investigations/threat-hunting-report.md` — LOLBin hunting documented
