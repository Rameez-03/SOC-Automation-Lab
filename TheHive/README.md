# TheHive – Case Management Configuration

## 📌 Overview

This folder contains all TheHive-related configuration files and screenshots used in the SOC Automation Lab.  
TheHive acts as the **Case Management** component, responsible for:

- Receiving enriched alerts automatically from Shuffle (SOAR)
- Creating and tracking incident cases for SOC analyst review
- Storing observables, TTPs, and attachments linked to each alert
- Providing an investigation dashboard for triage and response

---

## 📁 Folder Contents

| File | Description |
|------|-------------|
| `thehive.conf` | Main TheHive application configuration file |
| `TheHive-Application.jpg` | TheHive organisation list and instance overview |
| `Hive-Response.jpg` | Alerts list showing the auto-created Mimikatz case |
| `Hive-Response-Details.jpg` | Detailed view of the Mimikatz alert inside TheHive |

---

## 🖥️ TheHive Application

![TheHive Application](./TheHive-Application.jpg)

TheHive (version 5.5.14-2) is running as a single-organisation instance named **admin**, created by the TheHive system user on 21/01/2026. The organisation is active and serves as the central workspace for all SOC analyst investigations in this lab.

---

## ⚙️ Configuration – `thehive.conf`

TheHive is configured to run with all services hosted on the same dedicated cloud server.

### Database – Apache Cassandra
TheHive uses **Apache Cassandra** as its primary database backend via the JanusGraph CQL driver.

```
db.janusgraph {
  storage {
    backend = cql
    hostname = ["209.250.224.232"]
    cql {
      cluster-name = mydfir
      keyspace = thehive
    }
  }
}
```

- **Backend:** CQL (Cassandra Query Language)
- **Cluster Name:** mydfir
- **Keyspace:** thehive

### Index Engine – Elasticsearch
Elasticsearch is used for full-text search and alert indexing.

```
index.search {
  backend = elasticsearch
  hostname = ["209.250.224.232"]
  index-name = thehive
}
```

### File Storage
Attachments and case files are stored locally on the server filesystem.

```
storage {
  provider = localfs
  localfs.location = /opt/thp/thehive/files
}
```

### Service Settings
- **Base URL:** `http://209.250.224.232:9000`
- **Max attachment size:** 1GB
- **Max HTTP request body:** 10MB

### Additional Modules
TheHive is pre-integrated with **Cortex** (automated response) and **MISP** (threat intelligence sharing). Both modules are available but were not enabled in this lab configuration.

---

## 🚨 Auto-Created Alert – Mimikatz Detection

![Hive Response](./Hive-Response.jpg)

When Wazuh fired Rule ID 100002 and forwarded the alert to Shuffle, Shuffle automatically created a case in TheHive within seconds. The alert appears in the Alerts list with the following details:

| Field | Value |
|-------|-------|
| **Title** | Mimikatz Usage Detected |
| **Severity** | High |
| **Status** | New |
| **Source** | WAZUH Alert |
| **Reference** | 100002 |
| **Type** | internal |
| **MITRE TTP** | T1003 |
| **Created** | 24/01/2026 00:29 |

---

## 🔍 Alert Detail View

![Hive Response Details](./Hive-Response-Details.jpg)

Opening the alert reveals the full context automatically populated by Shuffle:

- **Title:** Mimikatz Usage Detected
- **Tags:** T1003 (OS Credential Dumping)
- **Description:** Mimikatz Usage Detected
- **Summary:** Mimikatz activity detected on host: Windows11
- **Source:** WAZUH Alert
- **Reference:** 100002
- **TLP:** AMBER
- **PAP:** AMBER
- **Severity:** HIGH
- **Detection Time:** < 1 second

The alert was created by `shuffle@test.com`, confirming the fully automated pipeline from Wazuh → Shuffle → TheHive with no manual intervention required.

---

## ✅ Summary

TheHive in this lab demonstrates automated case creation as part of a complete SOC pipeline:

- Alerts are **automatically ingested** from Shuffle via API with no manual effort
- Every alert is **pre-populated** with title, severity, TLP, MITRE TTPs, and a summary
- The SOC analyst receives a ready-to-triage case within seconds of the initial detection
- The platform supports further investigation via Observables, Responders, and History tabs

This makes TheHive a critical component in reducing analyst workload and accelerating incident response times.