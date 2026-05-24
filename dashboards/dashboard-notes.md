# Dashboard Screenshots

## encoded-powershell-alert.png
Alert fired for encoded PowerShell execution on Windows endpoint.
KQL rule: process.command_line: *EncodedCommand*
MITRE: T1059.001

## lolbin-hunt.png
Threat hunting session targeting LOLBin abuse —
legitimate Windows binaries used for malicious execution.
MITRE: T1218

## process-chain.png
Parent-child process relationship analysis —
WINWORD.EXE spawning powershell.exe confirmed malicious.
MITRE: T1566.001

## timeline-analysis.png
Full attack timeline reconstruction from initial 
execution to network connection attempt.

## powershell-detection.png
PowerShell execution events captured across endpoint.
KQL: process.name: "powershell.exe"

## logged-in-events.png
Authentication event monitoring — login activity 
across the endpoint during investigation period.

## fleet-agent.png
Elastic Fleet showing agent health — confirms 
successful telemetry collection from Windows endpoint.

## discover.png / discover-overview.png
Kibana Discover view showing live Windows endpoint 
telemetry with ECS normalised fields visible.

## powershell-logs.png
Raw PowerShell execution logs showing command line 
arguments captured by Elastic Agent.
