# Day 5 Notes — Threat Hunting Fundamentals

## Topics Learned

- threat hunting
- hunting methodology
- LOLBins
- attacker behavior analysis
- proactive investigations
- advanced KQL hunting

---

# What is Threat Hunting?

Threat hunting is the proactive process of searching telemetry for suspicious attacker behavior that may bypass automated alerts.

---

# LOLBins

LOLBins are legitimate Windows binaries abused by attackers.

Examples:
- powershell.exe
- certutil.exe
- rundll32.exe
- mshta.exe

---

# High-Value Hunt Areas

- encoded PowerShell
- Office spawning scripts
- failed logins
- suspicious process chains
- network activity

---

# Hunting Methodology

Hypothesis
→ Query
→ Investigation
→ Correlation
→ Documentation

---

# Key Lessons

Strong threat hunters:
- think behaviorally
- investigate context
- search proactively
- correlate events together
