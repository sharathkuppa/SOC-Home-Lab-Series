# Phase 1: Endpoint Security & Monitoring (Wazuh)

## Overview
This phase focuses on deploying a standalone **Wazuh SIEM** to monitor local endpoints. It demonstrates active vulnerability detection, remediation, and custom File Integrity Monitoring (FIM).

---

## Key Achievements

### 1. Vulnerability Remediation (CVE-2025-43859)
* **Detection:** Identified a critical vulnerability in the `h11` Python package using the Wazuh dashboard.
* **Action:** Executed a remediation cycle using PowerShell to upgrade the package and resolve `httpcore` dependency conflicts.
* **Verification:** Confirmed the alert cleared in the SIEM.

### 2. Custom File Integrity Monitoring (FIM)
* **Configuration:** Engineered custom monitoring rules by modifying the Windows agent's `ossec.conf` XML file.
* **Testing:** Validated rule triggers by simulating unauthorized file modifications.
* **Result:** Successfully captured Level 7 "Integrity checksum changed" alerts.
