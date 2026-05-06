# 🔐 Wazuh + MISP Integration (Threat Intelligence Enrichment)

## 📌 Overview

This project demonstrates the integration of **Wazuh SIEM** with **MISP (Malware Information Sharing Platform)** to perform real-time threat intelligence enrichment.

The integration extracts Indicators of Compromise (IOCs) such as:

* Malicious IP addresses
* Malicious domains
* File hashes (SHA256, MD5, SHA1)

These IOCs are queried against MISP, and if a match is found, Wazuh generates high-severity alerts.

---

## 🎯 Key Features

* 🔍 Automated IOC lookup via MISP API
* 🚨 Detection of malicious IPs, domains, and hashes
* ⚡ Real-time alert enrichment in Wazuh
* 🧠 SOC-ready correlation rules
* 🐍 Custom Python integration script

---

## 🏗️ Architecture

```
Endpoint (Sysmon Logs)
        ↓
    Wazuh Agent
        ↓
    Wazuh Manager
        ↓
Custom Integration Script (Python)
        ↓
       MISP API
        ↓
 Threat Intelligence Match
        ↓
   Enriched Alert Generated
```

---

## 📂 Project Structure

```
wazuh-misp-integration/
│
├── custom-misp.py
├── rules/
│   └── local_rules.xml
└── README.md
```

---

## ⚙️ Prerequisites

* Wazuh Manager installed
* Wazuh Agent with Sysmon (Windows/Linux)
* MISP server access
* Python 3 installed
* MISP API Key (Read-only recommended)

---

## 🚀 Setup Guide

### 1️⃣ Configure MISP

* Login to MISP
* Navigate to: **Automation → API Keys**
* Generate an API key
* Enable threat intelligence feeds

---

### 2️⃣ Configure Wazuh Integration

Edit:

```
/var/ossec/etc/ossec.conf
```

Add:

```xml
<integration>
  <name>custom-misp</name>
  <group>sysmon_event1,sysmon_event3,sysmon_event6,sysmon_event7,sysmon_event15,sysmon_event22,syscheck</group>
  <alert_format>json</alert_format>
</integration>
```

Restart Wazuh:

```bash
systemctl restart wazuh-manager
```

---

### 3️⃣ Add Integration Script

```bash
cp custom-misp.py /var/ossec/integrations/
chmod +x /var/ossec/integrations/custom-misp.py
```

---

### 4️⃣ Add Custom Rules

```bash
cp rules/local_rules.xml /var/ossec/etc/rules/
systemctl restart wazuh-manager
```

---

## 🧠 Detection Logic

The integration script:

1. Extracts IOC from Wazuh alerts (IP, domain, hash)
2. Sends request to MISP API
3. Checks for matching attributes
4. Generates enriched alert if match found

---

## 🧪 Testing

### Windows

```powershell
ping 8.8.8.8
nslookup malicious.com
```

### Linux

```bash
curl http://example.com
```

---

## 🚨 Sample Alerts

### Malicious IP

```
🚨 MISP: Malicious IP detected - 45.67.12.90
```

### Malicious Domain

```
🚨 MISP: Malicious Domain detected - badsite.com
```

### Malicious Hash

```
🚨 MISP: Malicious File Hash detected - <hash>
```

---

## 📊 Use Cases

* Threat intelligence enrichment
* SOC alert triaging
* IOC detection automation
* Malware investigation support

---


## 📚 Learning Outcomes

* SIEM + Threat Intelligence integration
* API-based automation in cybersecurity
* Real-world SOC workflow implementation

---

## ⚠️ Disclaimer

This project is for **educational purposes only**.

---

## 👨‍💻 Author

Jay Gohel
