# Day 4 Notes — SOC Investigation and Timeline Analysis

## Topics Learned

- event correlation
- timeline analysis
- phishing investigations
- attack chain reconstruction
- investigation workflow
- process chain analysis

---

# Timeline Analysis

Timeline analysis helps SOC analysts understand:
- what happened before the alert
- what triggered the alert
- what happened after the alert

---

# Parent-Child Process Analysis

Example:

WINWORD.EXE
→ powershell.exe

This behavior may indicate phishing or malicious macro activity.

---

# Event Correlation

Event correlation connects:
- processes
- users
- hosts
- authentication activity
- network connections

into one investigation story.

---

# Key Lessons

Strong SOC investigations focus on:
- behavior chains
- contextual analysis
- attack reconstruction
- evidence correlation
