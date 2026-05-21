# ECS Fields Notes

## What is ECS?

ECS (Elastic Common Schema) is a standardized field naming structure used in Elastic SIEM.

Different log sources generate logs in different formats. ECS converts those logs into a consistent structure so analysts can search and correlate data easily.

Without ECS:
- Different systems use different field names
- Queries become inconsistent
- Detection engineering becomes difficult

With ECS:
- Logs become normalized
- Queries work across multiple data sources
- Threat hunting becomes easier

---

# Why ECS is Important

ECS is critical for:
- Detection engineering
- Threat hunting
- Correlation
- Alert creation
- Dashboard building
- Investigation workflows

Example:

Different log formats:

```text
username
user
account
login_user
```

ECS normalizes them into:

```text
user.name
```

This consistency allows analysts to create reliable detection rules.

---

# Important ECS Fields

## 1. host.name

### Purpose
Identifies the system where the event occurred.

### Example

```text
host.name: DESKTOP-01
```

### Use Cases
- Identify affected systems
- Filter events by endpoint
- Investigate compromised hosts

---

## 2. user.name

### Purpose
Shows which user performed the activity.

### Example

```text
user.name: administrator
```

### Use Cases
- Track user activity
- Investigate compromised accounts
- Detect suspicious logins

---

## 3. process.name

### Purpose
Identifies the process executed on the endpoint.

### Example

```text
process.name: powershell.exe
```

### Use Cases
- Detect PowerShell abuse
- Identify suspicious tools
- Monitor command execution

---

## 4. process.command_line

### Purpose
Shows the full command executed.

### Example

```text
process.command_line: powershell.exe -EncodedCommand
```

### Use Cases
- Detect encoded commands
- Investigate attacker behavior
- Analyze malicious execution

---

## 5. source.ip

### Purpose
Identifies the source IP address initiating activity.

### Example

```text
source.ip: 192.168.1.10
```

### Use Cases
- Detect brute force attacks
- Track attacker origin
- Investigate lateral movement

---

## 6. destination.ip

### Purpose
Shows the destination IP address of network communication.

### Example

```text
destination.ip: 8.8.8.8
```

### Use Cases
- Detect suspicious outbound traffic
- Investigate command-and-control activity
- Monitor external communication

---

## 7. event.action

### Purpose
Describes the activity performed.

### Example

```text
event.action: logon-failed
```

### Use Cases
- Detect failed logins
- Monitor account activity
- Build authentication detections

---

## 8. event.category

### Purpose
Groups events into categories.

### Example

```text
event.category: process
```

### Common Categories
- authentication
- process
- network
- file
- registry

---

## 9. event.outcome

### Purpose
Shows whether the action succeeded or failed.

### Example

```text
event.outcome: failure
```

### Use Cases
- Failed login detection
- Authentication monitoring
- Error analysis

---

# ECS and Detection Engineering

Most detection rules rely on ECS fields.

Example detection:

```text
process.name: "powershell.exe"
```

This works because ECS standardizes process execution data.

Without ECS:
- Queries become inconsistent
- Detection rules break across systems

---

# Key Learning Points

- ECS standardizes security telemetry
- ECS improves threat hunting
- ECS enables consistent detections
- ECS is critical for SIEM operations
- Most Elastic queries depend on ECS fields

---

# Personal Notes

During this lab I used ECS fields to:
- Search PowerShell activity
- Investigate user actions
- Analyze process execution
- Understand log normalization
- Build detection queries
