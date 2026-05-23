# SOC Alert Triage Process

## Alert Workflow

Alert
→ Validation
→ Investigation
→ Severity Assessment
→ Escalation or Closure

---

# Investigation Questions

SOC analysts investigate:
- who executed the activity
- which host was affected
- parent-child process relationships
- network connections
- command line arguments
- timeline of activity

---

# Alert Severity Levels

## Low

Informational activity.

---

## Medium

Suspicious behavior requiring investigation.

---

## High

Likely malicious activity.

---

## Critical

Confirmed compromise or active attack.

---

# False Positives

A false positive occurs when legitimate activity is incorrectly identified as malicious.

False positives may create:
- alert fatigue
- missed attacks
- investigation delays

---

# Key Lessons

Effective SOC analysts:
- validate alerts carefully
- investigate behavior chains
- reduce false positives
- prioritize high-confidence alerts
