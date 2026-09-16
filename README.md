[README.md](https://github.com/user-attachments/files/32304600/README.md)
Homelab SOC / SIEM: Wazuh & Microsoft Sysmon
📋 Overview

A fully functional, containerized and virtualized Security Information and Event Management (SIEM) homelab built for educational and portfolio purposes. The lab simulates an enterprise-grade threat detection environment, integrating a Wazuh server on Ubuntu with a Windows 11 endpoint monitored via advanced telemetry (Microsoft Sysmon).

---

 🛠️ Architecture & Tech Stack
  * Host OS: Windows 11 Pro
  * Virtualization: Oracle VirtualBox (NAT with Port Forwarding for ports `8443`, `1514`, `1515`)
  * SIEM Platform: Wazuh (Manager & Dashboard deployed on Ubuntu 24.04 LTS Guest VM)
  * Endpoint Telemetry: Microsoft Sysmon (configured using SwiftOnSecurity ruleset)
  * Detection & Rules: Native Wazuh rules + MITRE ATT&CK framework mapping

---

 ⚙️ Implementation Steps

  1. Infrastructure & Network Setup
  * Deployed Ubuntu 24.04 LTS as a Guest VM inside VirtualBox.
  * Configured port forwarding to allow secure communication between the host and the SIEM manager:
  * 8443: Wazuh Web Dashboard
  * 1514 / 1515: Agent-to-Manager enrollment and communication protocols.

  2. Wazuh Agent Deployment & Verification
  Installed the Wazuh agent on the Windows 11 host machine (`Win11-Host`).
  Established a secure, authenticated channel verified via agent status (`Active`).

  3. Advanced Telemetry with Microsoft Sysmon
  Deployed **Sysmon64** using the production-ready `SwiftOnSecurity` configuration to capture deep kernel-level events (Process Creation, File Creation, Network Connections).
  Integrated Sysmon’s dedicated event channel into the Wazuh agent configuration (`ossec.conf`):
  ```xml
  <localfile>
    <location>Microsoft-Windows-Sysmon/Operational</location>
    <log_format>eventchannel</log_format>
  </localfile>
  ```

  🧪 Testing & Incident Detection
  To validate the SIEM's detection capabilities, several telemetry and security tests were conducted:

  Authentication Monitoring:

  Simulated failed login attempts (Event ID 4625), successfully triggering Wazuh rule logic (Logon Failure) and updating authentication failure metrics.

  Process & Script Execution Tracking:

  Validated real-time logging of PowerShell activity and process creation events through Sysmon integration.

  Monitored rule triggers mapped against the MITRE ATT&CK framework (e.g., detecting execution patterns and suspicious file drops).

  📊 Dashboard & Monitoring Highlights
  Active Endpoint Tracking: Real-time visibility of host health and event generation rates.

  Severity Classification: Automated categorization of alerts ranging from low-severity informational logs to critical-severity security alerts (Rule Level 15+).

  Threat Hunting: Querying raw telemetry using OpenSearch/DQL filters to investigate specific process GUIDs and command-line arguments.
