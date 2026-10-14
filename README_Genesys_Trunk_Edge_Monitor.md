# Project Name

**Genesys Cloud Trunk & Edge Health Monitor with Microsoft Teams Alerts**

---

# Overview

A Python-based monitoring solution that continuously checks the health of Genesys Cloud Trunks and Edges and sends real-time Microsoft Teams alerts whenever a service status changes.

The solution uses Genesys Cloud APIs, local state tracking, and Teams Adaptive Cards to provide proactive telephony monitoring.

---

# Objectives

- Monitor Genesys Cloud Trunks and Edges automatically
- Detect service outages and recoveries
- Send real-time Microsoft Teams notifications
- Reduce manual health checks
- Improve visibility of telephony infrastructure
- Maintain historical status information using local state files

---

# Features

- OAuth 2.0 authentication using Client Credentials
- Dynamic discovery of Trunks and Edges
- External Trunk filtering (`trunkType = EXTERNAL`)
- State change detection
- Microsoft Teams Adaptive Card alerts
- Recovery notifications
- Local JSON state tracking
- Rotating logfile management
- Environment variable based configuration
- Automatic polling and monitoring

---

# Architecture Flow

```text
+--------------------+
| Genesys Cloud APIs |
+----------+---------+
           |
           v
+--------------------+
|    monitor.py      |
+----------+---------+
           |
           v
+--------------------+
| State Comparison   |
| Current vs Old     |
+----------+---------+
           |
           v
+--------------------+
| Teams Webhook      |
| Power Automate     |
+----------+---------+
           |
           v
+--------------------+
| Teams Alert        |
| Channel            |
+--------------------+
```

---

# Workflow

```text
Read Environment Variables
           │
           ▼
Generate OAuth Token
           │
           ▼
Discover Trunks
           │
           ▼
Filter EXTERNAL Trunks
           │
           ▼
Collect Status

inService
enabled
connected
           │
           ▼
Discover Edges
           │
           ▼
Collect Status

state
           │
           ▼
Load state.json
           │
           ▼
Compare States
           │
           ▼
Status Changed?
      │
 ┌────┴────┐
 │         │
No        Yes
 │         │
 ▼         ▼
Exit   Send Teams Alert
             │
             ▼
      Update state.json
             │
             ▼
          Write Log
```

---

# Technology Stack

### Programming Language
- Python 3

### APIs
- Genesys Cloud REST API
- OAuth 2.0 (Client Credentials)

### Notifications
- Microsoft Teams Workflow Webhook
- Adaptive Cards

### Data Storage
- JSON (`state.json`)

### Logging
- Python Logging
- RotatingFileHandler

### Python Libraries
- requests
- python-dotenv
- urllib3
- json
- logging

---

# Teams Alert Example

### Trunk Failure Alert

```text
🚨 Genesys Trunk Alert

Resource Type: Trunk

Resource Name: KL SBC

Previous State:
inService = true
enabled = true
connected = true

Current State:
inService = false
enabled = true
connected = false

Suggested Check:
Admin > Telephony > Trunks
```

### Trunk Recovery Alert

```text
✅ Genesys Trunk Recovery

Resource Type: Trunk

Resource Name: KL SBC

Previous State:
inService = false
connected = false

Current State:
inService = true
connected = true
```

---

# Business Benefits

- Proactive fault detection
- Faster incident response
- Reduced Mean Time To Repair (MTTR)
- Real-time operational visibility
- Automated telephony health monitoring
- Improved service availability
- Reduced dependency on manual checks
- Instant notifications through Microsoft Teams
- Scalable design for multi-trunk and multi-edge environments
