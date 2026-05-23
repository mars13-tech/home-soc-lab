# Attack Timeline Analysis

## Timeline Reconstruction

### Stage 1 — Initial Activity

User interaction with suspicious document.

---

### Stage 2 — Process Execution

WINWORD.EXE launched PowerShell.

---

### Stage 3 — Command Execution

Encoded PowerShell command executed.

---

### Stage 4 — Potential Post-Exploitation

Reviewed:
- outbound network traffic
- child processes
- persistence activity

---

# Key Findings

The activity demonstrated a suspicious attack chain involving:
- Office execution
- PowerShell abuse
- possible phishing behavior

---

# Investigation Lessons

Attack investigations require:
- event correlation
- timeline analysis
- parent-child process analysis
- contextual investigation
