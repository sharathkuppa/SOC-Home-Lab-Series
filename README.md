# 🛡️ SOC Home Lab Series: Endpoint & Network Security

![Wazuh](https://img.shields.io/badge/Wazuh-00A9E5?style=for-the-badge&logo=wazuh&logoColor=white) ![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white) ![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white) ![Suricata](https://img.shields.io/badge/Suricata-EF3B2D?style=for-the-badge&logo=suricata&logoColor=white)

A comprehensive, multi-phase cybersecurity home lab designed to simulate Tier 1 Security Operations Center (SOC) environments. This repository documents the deployment, configuration, and active threat monitoring of both endpoint and network infrastructures.

## 🏗️ Lab Architecture

| Component | Technology | Purpose | Status |
| :--- | :--- | :--- | :--- |
| **Endpoint SIEM** | Wazuh | Vulnerability management, log aggregation, and File Integrity Monitoring (FIM). | ✅ Completed |
| **Network IDS/IPS** | Suricata | Packet capture, rule-based threat detection, and active network monitoring. | ✅ Completed |
| **Endpoints** | Windows 11, Ubuntu | Target machines for simulating attacks and monitoring anomalies. | 🔄 Active |

---

## 📁 Phase 1: Endpoint Security & Monitoring (Wazuh)
**View full documentation & configurations:** [01-Endpoint-Security-Wazuh](./01-Endpoint-Security-Wazuh/)

Deployed a standalone Wazuh manager and Windows agent to monitor a local endpoint for critical vulnerabilities and unauthorized file modifications.

**Key Achievements:**
* **Vulnerability Remediation (CVE-2025-43859):** Identified a critical vulnerability within the h11 Python package via the Wazuh dashboard. Executed a remediation cycle using PowerShell to upgrade the package, resolved httpcore dependency conflicts, and verified alert clearance.
* **Custom File Integrity Monitoring (FIM):** Engineered monitoring rules by modifying the Wazuh agent's `ossec.conf` XML configuration. Validated rule triggers for unauthorized file modifications, capturing Level 7 "Integrity checksum changed" alerts.

---

## 📁 Phase 2: Network Security & IDS Integration (Suricata + Wazuh)
**View full documentation & configurations:** [02-Network-IDS-Suricata](./02-Network-IDS-Suricata/)

Integrated a Suricata Network Intrusion Detection System (NIDS) on a Kali Linux sensor with the centralized Wazuh SIEM to monitor network traffic and detect malicious signatures in real time.

**Key Achievements:**
* **NIDS Deployment & JSON Logging:** Configured Suricata to log structured network telemetry to `/var/log/suricata/eve.json`.
* **SIEM Pipeline Integration:** Configured the Wazuh agent logcollector to ingest `eve.json` and transmit parsed logs to the Windows Docker-hosted Wazuh Manager across a VMware virtual bridge (`VMnet8`).
* **Threat Simulation & Triage:** Simulated a BlackSun Network Trojan using custom User-Agent strings. Verified rule `86601` alerts in the Wazuh Threat Hunting dashboard and parsed raw network metadata.

---

## 👨‍💻 Author
**Sharath K L**
* Security Operations Center (SOC) Analyst
* **LinkedIn:** [Sharath K L](https://www.linkedin.com/in/sharath-k-l-094660289)
* **Location:** Bengaluru, India
