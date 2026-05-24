# KQL Threat Hunting Queries — Elastic SIEM

**Author:** Karthikeyan  
**Lab:** Home SOC Lab — Elastic SIEM  
**Last Updated:** May 2026  
**Platform:** Kibana KQL / EQL  

---

## How to Use These Queries

1. Open Kibana → Discover
2. Select index pattern: `logs-*` or `winlogbeat-*`
3. Paste query into the KQL search bar
4. Set time range to your investigation window
5. Review results and pivot on suspicious fields

---

## 1. PowerShell Execution Hunting

### Basic PowerShell Detection

```kql
process.name: "powershell.exe"
```

> **Use:** Find all PowerShell activity on endpoint  
> **MITRE:** T1059.001 — Command and Scripting Interpreter: PowerShell  
> **Next step:** Filter by user.name to identify who ran it  

---

### Encoded PowerShell Detection

```kql
process.command_line: (*EncodedCommand* or *-enc* or *-e * or *-ec *)
```

> **Use:** Detect obfuscated PowerShell — high confidence malicious  
> **MITRE:** T1059.001, T1027 — Obfuscated Files or Information  
> **Next step:** Decode the Base64 argument to see what it executed  

---

### PowerShell with Suspicious Flags

```kql
process.command_line: (*bypass* or *hidden* or *noprofile* or *noninteractive* or *windowstyle hidden*)
```

> **Use:** Detect PowerShell launched to evade detection  
> **MITRE:** T1059.001, T1564 — Hide Artifacts  
> **Next step:** Check parent process and network connections  

---

### PowerShell Downloading from Internet

```kql
process.command_line: (*Invoke-WebRequest* or *IWR* or *wget* or *curl* or *DownloadString* or *DownloadFile*)
```

> **Use:** Detect PowerShell used as a downloader  
> **MITRE:** T1105 — Ingress Tool Transfer  
> **Next step:** Check destination IP or URL in the command line  

---

## 2. LOLBin Abuse Hunting

### All LOLBin Activity

```kql
process.name: ("certutil.exe" or "rundll32.exe" or "mshta.exe" or "wscript.exe" or "cscript.exe" or "regsvr32.exe" or "msiexec.exe" or "bitsadmin.exe")
```

> **Use:** Find all activity from commonly abused Windows binaries  
> **MITRE:** T1218 — Signed Binary Proxy Execution  
> **Next step:** Check parent process and command line arguments  

---

### LOLBin Spawned by Office Application

```kql
process.parent.name: ("winword.exe" or "excel.exe" or "powerpnt.exe" or "outlook.exe") and process.name: ("powershell.exe" or "cmd.exe" or "wscript.exe" or "mshta.exe" or "rundll32.exe")
```

> **Use:** Detect macro-based attacks — Office spawning shell or LOLBin  
> **MITRE:** T1566.001 — Phishing: Spearphishing Attachment  
> **Next step:** Identify the Office document that triggered the spawn  

---

### Certutil Used for Download

```kql
process.name: "certutil.exe" and process.command_line: (*urlcache* or *split* or *http*)
```

> **Use:** Detect certutil abused as a file downloader  
> **MITRE:** T1218.001, T1105  
> **Next step:** Extract the URL and check threat intel feeds  

---

### Regsvr32 Loading Remote Script

```kql
process.name: "regsvr32.exe" and process.command_line: (*scrobj* or *http* or */s* or */u*)
```

> **Use:** Detect Squiblydoo attack — regsvr32 loading remote COM scriptlet  
> **MITRE:** T1218.010  
> **Next step:** Check URL in command line, isolate if external  

---

## 3. Brute Force and Authentication Hunting

### Failed Logon Attempts

```kql
event.action: "logon-failed"
```

> **Use:** Detect failed authentication — baseline for brute force  
> **MITRE:** T1110 — Brute Force  
> **Next step:** Group by source.ip to find repeated failures  

---

### Multiple Failed Logons from Same Source

```kql
event.action: "logon-failed" and source.ip: *
```

> **Use:** Pivot on source IP to identify brute force source  
> **MITRE:** T1110  
> **Next step:** Count failures — 5+ from same IP in short window is suspicious  

---

### Successful Logon After Multiple Failures

```kql
event.action: "logon" and event.outcome: "success"
```

> **Use:** Look for successful login after failed attempts  
> **MITRE:** T1110.004 — Credential Stuffing  
> **Next step:** Correlate with failed logon timestamps from same user  

---

### Admin Account Logon

```kql
user.name: ("Administrator" or "admin" or "root") and event.action: "logon"
```

> **Use:** Monitor all privileged account activity  
> **MITRE:** T1078 — Valid Accounts  
> **Next step:** Verify if logon time and source are expected  

---

## 4. Process Chain Analysis

### Suspicious Parent-Child Relationships

```kql
process.parent.name: "winword.exe" and process.name: "powershell.exe"
```

> **Use:** Word spawning PowerShell — almost always malicious  
> **MITRE:** T1566.001  
> **Next step:** Identify the document, check command line  

---

### CMD Spawning Network Tools

```kql
process.parent.name: "cmd.exe" and process.name: ("net.exe" or "net1.exe" or "ping.exe" or "whoami.exe" or "ipconfig.exe")
```

> **Use:** Detect post-exploitation reconnaissance via CMD  
> **MITRE:** T1082, T1016  
> **Next step:** Check sequence of commands — recon pattern indicates active attacker  

---

### PowerShell Spawning Child Process

```kql
process.parent.name: "powershell.exe" and process.name: *
```

> **Use:** See what PowerShell is launching — reveals payload execution  
> **MITRE:** T1059.001  
> **Next step:** Review child process name and command line  

---

## 5. Network Connection Hunting

### PowerShell Making Network Connection

```kql
process.name: "powershell.exe" and destination.ip: *
```

> **Use:** Detect C2 communication via PowerShell  
> **MITRE:** T1071 — Application Layer Protocol  
> **Next step:** Check destination IP against threat intel feeds  

---

### Outbound Connection on Unusual Port

```kql
destination.port: (4444 or 1337 or 8080 or 9090 or 31337)
```

> **Use:** Common ports used by C2 frameworks — Metasploit default is 4444  
> **MITRE:** T1571 — Non-Standard Port  
> **Next step:** Identify process making connection, block if confirmed malicious  

---

### DNS Query Volume Spike

```kql
dns.question.name: *
```

> **Use:** Baseline DNS activity — spikes may indicate DNS tunneling  
> **MITRE:** T1071.004 — DNS  
> **Next step:** Filter by host and count unique DNS queries per minute  

---

## 6. Persistence Hunting

### Scheduled Task Created

```kql
event.action: "scheduled-task-created"
```

> **Use:** Detect persistence via scheduled tasks  
> **MITRE:** T1053.005 — Scheduled Task  
> **Next step:** Review task name, command, and user who created it  

---

### New Service Installed

```kql
event.action: "service-installed"
```

> **Use:** Detect persistence via Windows services  
> **MITRE:** T1543.003 — Windows Service  
> **Next step:** Review service binary path — malicious ones often point to temp directories  

---

### Registry Run Key Modification

```kql
registry.path: (*CurrentVersion\\Run* or *CurrentVersion\\RunOnce*)
```

> **Use:** Detect persistence via registry autorun keys  
> **MITRE:** T1547.001 — Registry Run Keys  
> **Next step:** Review what binary is being set to autorun  

---

## 7. Timeline Reconstruction

### All Events from Specific Host

```kql
host.name: "DESKTOP-XXXXX"
```

> **Use:** Pull everything from one endpoint during investigation window  
> **Next step:** Set time range to incident window — review chronologically  

---

### All Events from Specific User

```kql
user.name: "target-username"
```

> **Use:** Track all activity from a specific account  
> **Next step:** Look for unusual process execution, logons, or network activity  

---

### Full Process Execution Timeline

```kql
event.category: "process" and host.name: "DESKTOP-XXXXX"
```

> **Use:** Reconstruct complete process execution chain on one host  
> **Next step:** Sort by timestamp ascending — build attack timeline  

---

## MITRE ATT&CK Coverage Summary

| Query Category | Techniques Covered |
|---|---|
| PowerShell Hunting | T1059.001, T1027, T1105, T1564 |
| LOLBin Hunting | T1218, T1218.001, T1218.010, T1566.001 |
| Brute Force | T1110, T1110.004, T1078 |
| Process Chain | T1566.001, T1082, T1016, T1059.001 |
| Network | T1071, T1071.004, T1571 |
| Persistence | T1053.005, T1543.003, T1547.001 |
| Timeline | Multiple |

---

## Connected Files in This Lab

- `detection-rules/encoded-powershell.md` — rule using PowerShell queries
- `detection-rules/brute-force.md` — rule using authentication queries
- `detection-rules/word-powershell.md` — rule using process chain queries
- `detection-rules/lolbin-detection.md` — rule using LOLBin queries
- `investigations/encoded-powershell-investigation.md` — full walkthrough
- `investigations/threat-hunting-report.md` — hunting session documented
- `dashboards/lolbin-hunt.png` — LOLBin hunt screenshot
- `dashboards/process-chain.png` — process chain screenshot
