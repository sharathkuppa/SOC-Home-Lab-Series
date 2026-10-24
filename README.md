# 🛡️ SOC Home Lab Series

![Wazuh](https://img.shields.io/badge/Wazuh-00A9E5?style=for-the-badge&logo=wazuh&logoColor=white) ![Suricata](https://img.shields.io/badge/Suricata-EF3B2D?style=for-the-badge&logo=suricata&logoColor=white) ![Windows](https://img.shields.io/badge/Windows_11-0078D6?style=for-the-badge&logo=windows&logoColor=white) ![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white) ![Kali](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)

A five-phase home lab that simulates a Tier 1 SOC workflow: **detect → triage → respond**. Each phase has its own write-up, the exact configuration files used, and screenshots of the results.

## 🏗️ Lab Architecture

| Component | Technology | Role |
| :--- | :--- | :--- |
| SIEM | Wazuh (Manager, Indexer, Dashboard) in Docker on Windows | Log aggregation, vulnerability detection, FIM, alerting, active response |
| Network IDS | Suricata on Kali Linux | Signature-based network detection, JSON telemetry (`eve.json`) |
| Endpoints | Windows 11, Ubuntu (WSL) | Monitored targets |
| Attacker | Kali Linux (VMware, VMnet8) | Simulated attacks (Hydra, crafted HTTP requests) |

## 📚 Phases

| # | Phase | What it demonstrates | Key detections |
| :-: | :--- | :--- | :--- |
| 1 | [Endpoint Security & Monitoring](./01-endpoint-security-wazuh/) | Vulnerability management, remediation, custom FIM | CVE-2025-43859 (h11), Level 7 "Integrity checksum changed" |
| 2 | [Network IDS Integration](./02-network-ids-suricata/) | Suricata → Wazuh log pipeline, alert triage | Rule 86601 (ET USER_AGENTS "BlackSun") |
| 3 | [Automated Active Response](./03-active-response-wazuh/) | Detection-to-containment automation | Rule 5716 → `firewall-drop` → Rule 651 |
| 4 | [Sysmon Log Analysis](./04-sysmon-log-analysis/) | Host telemetry, detection coverage testing, alert triage | Rules 92057, 92031/92033, 92302; 2 detection gaps documented |
| 5 | [Custom Detection Rules](./05-custom-rules/) | Writing and tuning Wazuh rules, debugging rule-chain shadowing | Rules 100010, 100011, 100012; closed 2 detection gaps, tuned 1 false positive |

## 🔎 Highlights

- **Remediated a critical CVE end to end:** found it in the Wazuh dashboard, upgraded the package with PowerShell, resolved the `httpcore` dependency conflict, confirmed the alert cleared.
- **Built custom FIM rules** in the Windows agent's `ossec.conf` and validated them with simulated file tampering.
- **Wired Suricata into Wazuh** by ingesting `eve.json` through the agent's `logcollector` across a VMware virtual network.
- **Automated containment:** a Hydra SSH brute-force attack triggers Wazuh Rule 5716, which runs `firewall-drop` and adds the attacker's IP to the iptables drop list for 60 seconds.
- **Tested detection coverage with Sysmon:** simulated five attacker behaviours, found 3 detected and 2 gaps, confirmed the gaps were in Wazuh's ruleset rather than Sysmon logging, and triaged two high-severity alerts (rules 92213 and 92058) as false positives using process, parent and command-line evidence.
- **Wrote custom Wazuh rules** to close the two Sysmon detection gaps (scheduled-task creation, PowerShell outbound connections) and to tune a level-15 false positive down to level 3 without disabling the original rule. When one rule did not fire, traced the cause through logging, delivery and rule evaluation to a built-in level-0 rule (92101) that was consuming the event first.

## 📁 Repository Structure

```
soc-home-lab-series/
├── 01-endpoint-security-wazuh/
│   ├── README.md
│   ├── configs/windows-agent-ossec.conf
│   └── screenshots/
├── 02-network-ids-suricata/
│   ├── README.md
│   ├── configs/wazuh-agent-suricata-localfile.xml
│   └── screenshots/
├── 03-active-response-wazuh/
│   ├── README.md
│   ├── configs/active-response-firewall-drop.xml
│   └── screenshots/
├── 04-sysmon-log-analysis/
│   ├── README.md
│   ├── configs/wazuh-agent-sysmon-localfile.xml
│   └── screenshots/
├── 05-custom-rules/
│   ├── README.md
│   ├── configs/sysmon_custom_rules.xml
│   └── screenshots/
├── LICENSE
└── README.md
```

## ⚠️ Disclaimer

All attacks were run against systems I own, inside an isolated lab network, for learning purposes only.

## 👨‍💻 Author

**Sharath K L**, aspiring SOC / Security Analyst, Bengaluru, India
[LinkedIn](https://www.linkedin.com/in/sharath-k-l-094660289) · [GitHub](https://github.com/sharathkuppa)
