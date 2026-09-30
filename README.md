# Wazuh SIEM Home Lab — File Integrity Monitoring

## Overview
This project documents the deployment of **Wazuh**, a free and open-source
Security Information and Event Management (SIEM) and Extended Detection and
Response (XDR) platform, in a home lab environment. The goal was to build a
working SIEM setup capable of collecting, analyzing, and alerting on
security-relevant events from an endpoint, with a focus on **File Integrity
Monitoring (FIM)** — detecting real-time file creation, modification, and
deletion on a monitored system.

## Architecture

| Component      | Host                          | Role                                                              |
|-----------------|-------------------------------|--------------------------------------------------------------------|
| Wazuh Manager   | Ubuntu Server 22.04 (VMware)  | Collects, indexes, and analyzes data from agents; hosts the dashboard |
| Wazuh Agent     | Windows 11 (host machine)     | Sends system logs and file-integrity events to the manager        |

The manager and agent communicate over a NAT-based virtual network, with the
agent forwarding real-time syscheck events to the manager for rule matching
and visualization via the Wazuh web dashboard.

## Objectives
- Deploy a complete Wazuh stack (indexer, manager, dashboard) on Ubuntu Server
- Install and register a Wazuh agent on a Windows 11 endpoint
- Configure real-time File Integrity Monitoring on a test directory
- Generate and verify FIM events, and interpret the resulting alert data as a SOC analyst would

## What's Included
- `SIEM_WAZUH_LAB.docx` — full step-by-step lab documentation, including
  configuration commands, screenshots, and troubleshooting notes
- Screenshots of the Wazuh dashboard showing an active agent and captured FIM events

## Key Steps Performed
1. Provisioned an Ubuntu Server 22.04 VM in VMware (NAT networking)
2. Installed the full Wazuh stack (indexer, manager, dashboard) via the official install script
3. Installed and registered the Wazuh agent on the Windows 11 host
4. Configured `ossec.conf` to monitor a test directory in real time
5. Triggered and verified FIM events (added / modified / deleted), each mapped
   to its Wazuh rule ID (554 / 550 / 553)

## Challenges Faced
- **Indexer install timeout:** the Wazuh indexer package (~875MB) failed to
  download during installation due to a network timeout; resolved by
  re-running the install script
- **Agent connectivity failure:** the agent showed "Running" locally but
  never appeared as connected on the manager; diagnosed via the agent's
  `ossec.log`, which revealed a stray whitespace character in the configured
  manager IP address, causing a hostname resolution failure. Fixed by
  re-entering the IP cleanly and restarting the agent

## Skills Demonstrated
- SIEM/XDR deployment and configuration
- Agent-manager registration and troubleshooting
- File Integrity Monitoring configuration (`ossec.conf` / syscheck)
- Log and event analysis from a SOC analyst perspective
- Root-cause diagnosis of connectivity issues using log files

## Notes for Production Use
This lab uses broad, illustrative monitoring for demonstration purposes. In a
production deployment, FIM scope would be narrowed to high-value targets
(e.g. system configuration files, application directories, web server roots)
rather than a general-purpose directory, to reduce alert noise and keep
analyst attention on meaningful changes.

## Reference
Guide followed: *WAZUH Home Lab – SIEM and File Integrity Monitoring* by
Royden Rebello (The Social Dork)
