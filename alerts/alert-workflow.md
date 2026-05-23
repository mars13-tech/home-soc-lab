# SOC Alert Workflow

## Alert Lifecycle

Alert
→ Validation
→ Investigation
→ Severity Assessment
→ Escalation or Closure

---

# Alert Validation

SOC analysts validate:
- whether activity is malicious
- affected users
- affected hosts
- command execution details
- related activity

---

# Investigation Process

Analysts review:
- parent-child processes
- command line arguments
- network activity
- authentication activity
- timeline correlation

---

# Severity Levels

## Low

Informational activity.

---

## Medium

Suspicious activity requiring investigation.

---

## High

Likely malicious behavior.

---

## Critical

Confirmed compromise or active attack.

---

# False Positives

A false positive occurs when legitimate activity is incorrectly identified as malicious.

False positives create:
- alert fatigue
- investigation delays
- reduced analyst efficiency

---

# Key Lessons

Effective SOC operations require:
- strong detections
- proper tuning
- investigation context
- efficient triage workflows
