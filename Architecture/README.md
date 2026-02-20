# SOC Automation Lab – Architecture Overview

## 📌 Overview

This document outlines the architecture of the SOC Automation Lab. The environment was designed to simulate a real-world Security Operations Center (SOC) pipeline integrating SIEM, SOAR, Case Management, and automated incident response.

The lab demonstrates how alerts are generated, enriched, escalated, and responded to automatically.

---

## 🏗️ High-Level Architecture

![SOC Architecture](soc-architecture.png)

### Workflow Summary

1. Windows 10 client generates security events.
2. Wazuh Agent forwards logs to the Wazuh Manager.
3. Wazuh Manager analyzes logs and triggers alerts.
4. Alerts are forwarded to Shuffle (SOAR platform).
5. Shuffle enriches Indicators of Compromise (IOCs) using OSINT sources.
6. Shuffle creates a case in TheHive.
7. Email notification is sent to the SOC Analyst.
8. Upon approval, a response action is triggered.
9. Wazuh executes active response on the Windows endpoint.

---

## ☁️ Cloud Infrastructure

The lab components were deployed using separate Ubuntu cloud servers to simulate a distributed SOC environment.

---

### 🛡️ Wazuh Server

**Purpose:** SIEM + Log Management + Active Response

**Specifications:**
- OS: Ubuntu 24.04 LTS (x64)
- vCPUs: 4
- RAM: 8GB
- Storage: 160GB SSD
- Location: London

**Responsibilities:**
- Receive logs from Windows Wazuh Agent
- Apply detection rules
- Generate alerts
- Send alerts to Shuffle
- Execute active response commands

---

### 🧠 TheHive Server

**Purpose:** Case Management Platform

**Specifications:**
- OS: Ubuntu 24.04 LTS (x64)
- vCPUs: 6
- RAM: 16GB
- Storage: 320GB SSD
- Location: London

**Responsibilities:**
- Receive enriched alerts from Shuffle
- Create and manage incident cases
- Provide investigation interface for SOC Analyst
- Store artifacts and observables

---

## 🔁 Data Flow Breakdown

### 1️⃣ Endpoint Telemetry Collection
- Windows 10 machine runs Wazuh Agent
- Security events are generated (e.g., suspicious activity)
- Logs are sent to Wazuh Manager

### 2️⃣ Detection & Alert Generation
- Wazuh applies built-in and custom rules
- Alerts are triggered when rule conditions are met

### 3️⃣ SOAR Automation (Shuffle)
- Receives alert via webhook/API
- Extracts IOCs (IP, hash, domain, etc.)
- Enriches data using OSINT integrations
- Decides response workflow

### 4️⃣ Case Creation (TheHive)
- Shuffle pushes alert data into TheHive
- A new case is automatically created
- Observables are attached

### 5️⃣ Notification & Response
- Email notification sent to SOC Analyst
- Analyst reviews alert
- If malicious → response action executed
- Wazuh performs active response on endpoint

---

## 🎯 Architectural Design Goals

This architecture was designed to:

- Simulate a real SOC environment
- Automate alert enrichment
- Reduce analyst workload
- Demonstrate SIEM-to-SOAR integration
- Enable automated containment actions
- Provide full alert lifecycle visibility

---

## 🔐 Security Considerations

- Servers are separated to mimic production environments
- API keys are used for integrations
- Webhooks are configured for secure communication
- Active response is controlled via rule logic

---

## 📈 SOC Automation Pipeline

Windows Client  
↓  
Wazuh (SIEM & Detection)  
↓  
Shuffle (SOAR & Enrichment)  
↓  
TheHive (Case Management)  
↓  
SOC Analyst  
↓  
Automated Response  
↓  
Endpoint Containment  

---

## 🚀 Summary

This SOC Automation Lab demonstrates a complete detection-to-response pipeline:

- Log ingestion
- Alert detection
- Automated enrichment
- Case creation
- Analyst notification
- Active response execution

The architecture reflects modern SOC design principles integrating SIEM, SOAR, and case management into a unified automated workflow.