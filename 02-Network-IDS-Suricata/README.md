\# Phase 2: Network Security \& IDS Integration (Suricata + Wazuh)



\## Overview

This phase expands detection capabilities from host-level monitoring to network intrusion detection by integrating \*\*Suricata NIDS\*\* with the \*\*Wazuh SIEM\*\*. Malicious network events detected on a Kali Linux sensor are parsed into structured JSON and ingested in real time into the centralized Wazuh Manager.



\---



\## Network Architecture \& Data Flow



\- \*\*Sensor (Kali Linux)\*\*: Captures network packets and writes alerts to `/var/log/suricata/eve.json`.

\- \*\*Host (Windows Docker)\*\*: Runs single-node Wazuh Manager, Indexer, and Dashboard.

\- \*\*Network Bridge\*\*: VMware NAT adapter (`VMnet8`, `192.168.125.0/24`).



\---



\## Key Configurations



\### Wazuh Agent Log Ingestion (`ossec.conf`)

Added the Suricata JSON log collection block to the agent configuration:---



\## Attack Simulation \& Verification



1\. \*\*Triggering Trojan Traffic\*\*:

&#x20;  Executed an HTTP request using a known malicious user-agent string (Emerging Threats signature ID `2008983`):

&#x20;  `curl -A "BlackSun" http://example.com`



2\. \*\*SIEM Incident Triage\*\*:

&#x20;  - Wazuh triggered internal rule `86601` (`Suricata: Alert - ET USER\_AGENTS Suspicious User Agent (BlackSun)`).

&#x20;  - Extracted alert metadata: Category `A Network Trojan was detected`, Severity `1`, Destination Port `80`.

