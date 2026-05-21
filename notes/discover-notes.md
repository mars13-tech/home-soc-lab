# Kibana Discover Notes

## Objective

The purpose of this exercise was to explore endpoint telemetry in Kibana Discover and understand how logs are stored, searched, and investigated in Elastic SIEM.

---

# What is Kibana Discover?

Kibana Discover is used to:
- Search logs
- Filter events
- Investigate activity
- Explore ECS fields
- Perform threat hunting

It is one of the most important tools used by SOC analysts.

---

# Initial Verification

After installing Elastic Agent and connecting it to Elastic Cloud, logs were successfully visible in Discover.

This confirmed:
- Agent connectivity
- Data ingestion
- Elasticsearch indexing
- ECS normalization

---

# Queries Tested

## 1. PowerShell Execution

Query:

```text
process.name: powershell.exe
```

### Result

Observed PowerShell execution events from the local Windows endpoint.

### Learning

Process execution data is stored using ECS fields such as:
- process.name
- process.command_line
- user.name
- host.name

---

## 2. CMD Execution

Query:

```text
process.name: cmd.exe
```

### Result

Observed command prompt activity generated locally.

### Learning

Different process executions can be filtered using ECS process fields.

---

## 3. Failed Login Activity

Query:

```text
event.action: logon-failed
```

### Result

Observed failed authentication attempts.

### Learning

Authentication events are categorized using ECS event fields.

---

## 4. Network Activity

Query:

```text
destination.ip: *
```

### Result

Observed outbound network communication events.

### Learning

Elastic stores network telemetry using source and destination fields.

---

# Important ECS Fields Observed

## host.name

Identifies the system generating logs.

---

## user.name

Shows the user responsible for activity.

---

## process.name

Identifies executed processes.

---

## process.command_line

Shows full command execution details.

---

## source.ip

Shows source network address.

---

## destination.ip

Shows destination network address.

---

## event.action

Describes activity performed.

---

# Key Observations

## 1. Logs Become Searchable Through ECS

Raw Windows logs are normalized into ECS format.

Example:

Raw Log:

```text
New Process Name: powershell.exe
```

Normalized:

```text
process.name: powershell.exe
```

---

## 2. Threat Hunting Becomes Easier

Using ECS fields allows faster searches across multiple log types.

---

## 3. Kibana Discover is Critical for SOC Work

SOC analysts use Discover for:
- investigations
- alert validation
- threat hunting
- timeline analysis

---

# Challenges Faced

## Initial Confusion About ECS

At first it was difficult to understand how raw Windows logs become ECS fields.

After exploring Discover, it became clear that ingest pipelines normalize events automatically.

---

## Understanding Field Relationships

Some fields initially appeared confusing because multiple ECS fields describe different parts of an event.

Example:
- process.name
- process.command_line
- event.category

---

# Key Learning Outcomes

- Learned how logs appear inside Discover
- Understood ECS field normalization
- Performed basic threat hunting
- Explored endpoint telemetry
- Verified successful data ingestion
- Improved understanding of SIEM workflows

---

# Personal Reflection

This exercise helped build a strong foundation in:
- SIEM architecture
- Log analysis
- ECS normalization
- Kibana Discover usage
- Threat hunting basics

Understanding Discover is essential because most SOC investigations begin by searching and filtering telemetry data.
