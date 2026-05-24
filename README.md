# Home SOC Lab — Elastic SIEM

**Author:** Karthikeyan  
**Target Role:** SOC Analyst (Blue Team)  
**Platform:** Elastic Cloud · Elasticsearch · Kibana · Elastic Agent  
**Endpoint:** Windows 11  
**Status:** Active  
🔗 [LinkedIn](https://www.linkedin.com/in/karthi-keyan-9042862bb) · 🐙 [GitHub](https://github.com/mars13-tech)

---

## What This Is

A fully working home SOC lab built on Elastic SIEM from scratch.

Real Windows endpoint telemetry. Real detection rules. Real alerts. Real investigations.

Built to develop the practical skills an L1/L2 SOC analyst uses every shift — not to follow a tutorial, but to understand what happens between an alert firing and a ticket closing.

---

## Lab Architecture

```
Windows 11 Endpoint
        ↓
Elastic Agent (Fleet managed)
        ↓
Elastic Cloud (Elasticsearch)
        ↓
Kibana (Discover · Alerts · Timeline)
        ↓
Detection Rules (KQL / EQL)
        ↓
Security Alerts → Investigation → Response
```

---

## Repository Structure

```
home-soc-lab/
│
├── README.md
│
├── setup/
│   ├── elastic-cloud-setup.md       — Elastic Cloud deployment walkthrough
│   ├── agent-install.md             — Fleet agent installation on Windows
│   └── troubleshooting.md           — Real issues hit and how they were fixed
│
├── architecture/
│   ├── data-flow.md                 — End-to-end data flow explanation
│   └── siem-architecture.png        — Visual architecture diagram
│
├── dashboards/
│   ├── discover.png                 — Kibana Discover live telemetry view
│   ├── discover-overview.png        — Full Discover dashboard overview
│   ├── encoded-powershell-alert.png — Alert fired for encoded PowerShell
│   ├── fleet-agent.png              — Fleet agent health confirmed
│   ├── logged-in-events.png         — Authentication event monitoring
│   ├── lolbin-hunt.png              — LOLBin threat hunting session
│   ├── powershell-detection.png     — PowerShell execution detection
│   ├── powershell-logs.png          — Raw PowerShell logs captured
│   ├── process-chain.png            — Parent-child process analysis
│   └── timeline-analysis.png        — Full attack timeline reconstruction
│
├── notes/
│   ├── day2-learning-notes.md       — Elastic Cloud setup, first agent connection
│   ├── day3-notes.md                — ECS fields, Kibana Discover navigation
│   ├── day4-notes.md                — First KQL queries, PowerShell log analysis
│   ├── day5-notes.md                — Detection rule creation, alert configuration
│   ├── day6-notes.md                — Alert tuning, false positive reduction
│   ├── discover-notes.md            — Kibana Discover workflow reference
│   └── ecs-fields.md                — ECS field reference for SOC analyst use
│
├── detection-rules/
│   ├── encoded-powershell.md        — T1059.001 — Encoded PowerShell detection
│   ├── brute-force.md               — T1110 — Brute force login detection
│   └── word-powershell.md           — T1566.001 — Office macro spawning PowerShell
│
├── queries/
│   ├── kql-detections.md            — KQL queries tied to detection rules
│   └── kql-threat-hunting.md        — KQL queries for active threat hunting
│
├── investigations/
│   ├── alert-triage-process.md      — L1 analyst alert triage workflow
│   ├── attack-timeline.md           — Full attack timeline reconstruction
│   ├── case-management-notes.md     — Ticket and case lifecycle documentation
│   ├── encoded-powershell-investigation.md — Full encoded PowerShell walkthrough
│   ├── incident-response-report.md  — Formal IR report
│   ├── phishing-investigation.md    — Phishing attack investigation
│   └── threat-hunting-report.md     — Proactive threat hunting session
│
└── alerts/
    ├── alert-tuning.md              — False positive reduction documentation
    └── alert-workflow.md            — End-to-end alert handling process
```

---

## Detection Rules

| Rule | MITRE Technique | ID | Severity |
|---|---|---|---|
| Encoded PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 | High |
| Brute Force Login Attempts | Brute Force | T1110 | Medium |
| Office Application Spawning PowerShell | Phishing: Spearphishing Attachment | T1566.001 | High |
| LOLBin Abuse | Signed Binary Proxy Execution | T1218 | High |

---

## Key KQL Queries

### Encoded PowerShell Detection
```kql
process.command_line: (*EncodedCommand* or *-enc* or *-e * or *-ec *)
```

### LOLBin Abuse Detection
```kql
process.name: ("certutil.exe" or "rundll32.exe" or "mshta.exe" or "wscript.exe" or "cscript.exe" or "regsvr32.exe")
```

### Office Spawning PowerShell
```kql
process.parent.name: ("winword.exe" or "excel.exe" or "powerpnt.exe") and process.name: "powershell.exe"
```

### Brute Force Detection
```kql
event.action: "logon-failed"
```

> Full query library → [queries/kql-threat-hunting.md](./queries/kql-threat-hunting.md)

---

## Investigations Completed

| Investigation | Attack Type | MITRE |
|---|---|---|
| Encoded PowerShell Investigation | PowerShell obfuscation | T1059.001, T1027 |
| Phishing Investigation | Office macro execution | T1566.001 |
| Threat Hunting Report | LOLBin abuse hunting | T1218 |
| Attack Timeline Reconstruction | Full kill chain analysis | Multiple |
| Alert Triage Process | L1 analyst workflow | — |
| Case Management Notes | Ticket lifecycle | — |
| Incident Response Report | Formal IR documentation | Multiple |

---

## ECS Fields Used in This Lab

| ECS Field | Purpose |
|---|---|
| `host.name` | Identifies affected system |
| `user.name` | Identifies user activity |
| `process.name` | Identifies executed process |
| `process.command_line` | Shows full command execution |
| `process.parent.name` | Shows parent process — critical for chain analysis |
| `source.ip` | Shows source IP address |
| `destination.ip` | Shows destination IP |
| `event.action` | Describes performed action |
| `event.category` | Categorizes event type |
| `event.outcome` | Shows success or failure |

---

## Skills Demonstrated

| Skill | Evidence |
|---|---|
| SIEM Deployment | setup/ — full Elastic Cloud setup documented |
| Detection Engineering | detection-rules/ — 3 rules with MITRE mapping |
| KQL Threat Hunting | queries/ — 20+ hunting queries across 7 categories |
| Alert Tuning | alerts/alert-tuning.md — false positive reduction |
| Incident Investigation | investigations/ — 7 investigation files |
| Log Analysis | notes/ — 7 days of progressive learning notes |
| Architecture Understanding | architecture/ — data flow + diagram |
| ECS Normalization | notes/ecs-fields.md — field reference documented |

---

## What I Learned That Wasn't in Any Tutorial

Writing a detection rule took 20 minutes.

Tuning it so it does not fire on legitimate PowerShell activity took much longer.

A detection that fires on everything is noise. Noise gets ignored. Ignored alerts miss real attacks. That is how breaches happen.

The other thing — debugging why logs were not ingesting taught me more about how Elastic Agent and Elasticsearch talk to each other than any documentation did.

---

## Future Improvements

- [ ] Sysmon integration for deeper endpoint visibility
- [ ] Sigma rule conversion for cross-platform detection
- [ ] Advanced EQL detections for sequence-based attacks
- [ ] Threat intelligence feed integration
- [ ] Linux endpoint telemetry
- [ ] Automated response workflows

---

## Author

**Karthikeyan**  
Cybersecurity Engineering Student | Blue Team | SOC Analyst in the Making  
🔗 [LinkedIn](https://www.linkedin.com/in/karthi-keyan-9042862bb)  
🐙 [GitHub](https://github.com/mars13-tech)
