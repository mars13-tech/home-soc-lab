# Brute Force Detection

## Detection Objective

Detect repeated failed login attempts that may indicate brute force attacks or password spraying.

---

# Rule Type

Threshold Rule

---

# KQL Query

```kql
event.action: "logon-failed"
```

---

# Threshold Logic

Alert when:
- 5 failed logins occur
- within 2 minutes

---

# Severity

Medium

---

# Investigation Guide

Investigate:
- source.ip
- targeted accounts
- login timing
- affected hosts
- repeated authentication failures

---

# Why This Detection Matters

Repeated failed logins may indicate:
- brute force attacks
- password spraying
- unauthorized access attempts

Attackers commonly use automated tools to attempt multiple passwords against user accounts.

---

# Potential False Positives

Possible legitimate causes:
- user forgot password
- misconfigured applications
- outdated credentials
- service account issues

---

# Key Lessons

Threshold detections help identify:
- repeated attacker behavior
- abnormal authentication patterns
- automated attack activity
