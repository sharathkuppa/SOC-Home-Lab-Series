# Wazuh Active Response Lab: SSH Brute-Force Automated Defense

## 🎯 Objective
Configured a Wazuh SIEM environment to detect SSH brute-force attacks (Hydra) and automatically trigger an Active Response (firewall-drop) to block the attacker's IP using iptables.

## 🏗️ Lab Architecture

| Component | Technology | Purpose | Status |
| :--- | :--- | :--- | :--- |
| **Endpoint SIEM** | Wazuh | Vulnerability management, log aggregation, and File Integrity Monitoring (FIM). | ✅ Completed |
| **Network IDS/IPS** | Suricata | Packet capture, rule-based threat detection, and active network monitoring. | ✅ Completed |
| **Endpoints** | Windows 11, Ubuntu | Target machines for simulating attacks and monitoring anomalies. | 🔄 Active |

## 🛠️ Attack & Defense Flow
1. **Attack:** Kali Linux VM launches a Hydra SSH brute-force attack against an Ubuntu WSL endpoint.
2. **Detection:** The Wazuh Agent forwards syslogs to the Wazuh Manager, which matches Rule 5716 (sshd: authentication failed).
3. **Response:** The Manager triggers the <active-response> script.
4. **Execution:** The Ubuntu endpoint executes firewall-drop, adding the attacker's IP to the iptables drop list for 60 seconds.
