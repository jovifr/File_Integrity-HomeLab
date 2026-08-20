# File_Integrity-HomeLab
Built a home SOC lab using Wazuh for SIEM and real-time file integrity monitoring.
# Wazuh Home Lab — SIEM & File Integrity Monitoring

## Overview
Two-host Wazuh SIEM lab built to practice log collection, endpoint agent management, and real-time File Integrity Monitoring (FIM) detection verification.

## Architecture

| Component | Host | Role |
|---|---|---|
| Wazuh Manager | Kali Linux (VirtualBox, bridged network) | Collects, analyzes, and stores agent data |
| Wazuh Agent | Windows 11 Pro (physical host) | Sends logs and file-system events to the manager |

## Setup

1. Installed the Wazuh manager on Kali Linux via the official install script (`wazuh-install.sh -a -i`), verified against the Wazuh GPG key.
2. Accessed the Wazuh dashboard over HTTPS on the manager's bridged IP.
3. Registered the Windows agent using `manage_agents`, then applied the enrollment key through the Wazuh Agent Manager GUI.
4. Enabled real-time FIM by adding a monitored directory to the `<directories realtime="yes">` block in `ossec.conf`.
5. Restarted the agent service and confirmed Active status on the dashboard.

## Verification

- Agent confirmed **Active** in the Endpoints dashboard (Windows 11 Pro, default group).
- Triggered file create/delete events in the monitored directory.
- Dashboard captured 7 FIM events in a 24-hour window:
  - `rule.id 554` (level 5) — file added
  - `rule.id 553` (level 7) — file deleted
- Detection latency confirmed: events appeared in the dashboard within the same 30-minute window as the triggering action.

## Troubleshooting

Resolved an agent/manager version mismatch (Windows agent on a newer minor release than the 4.12.0 manager) by aligning agent and manager versions — a common real-world Wazuh deployment issue.

## Skills Demonstrated

Linux server administration, SIEM deployment, Windows endpoint agent management, FIM/Syscheck configuration, detection verification, dashboard-based log analysis.
