# Home SOC Lab

## Overview
This project documents a home Security Operations Center (SOC) lab built to simulate real-world alert detection and incident response.

## Tools Used
- VirtualBox
- Windows 10 (Victim)
- Ubuntu Linux (Attacker)
- Wazuh SIEM
- Sysmon
- MITRE ATT&CK

## Lab Architecture
Going to insert SS/diagrams 

## Detection Scenarios
- Brute force login detection
- Suspicious PowerShell execution
- Credential access simulation

## What This Demonstrates
- Log analysis
- Alert triage
- Incident investigation
- SOC analyst methodology

--------------------------------

## Wazuh Deployment Notes

### Environment
- Ubuntu Server running in VirtualBox
- Wazuh all-in-one deployment (Manager, Indexer, Dashboard)

### Installation Process
- Installed Wazuh using the official installation script
- Verified OpenSearch cluster health (GREEN)
- Confirmed Wazuh Dashboard accessibility via https://127.0.0.1

### Issues Encountered & Fixes
- Encountered OpenSearch security permission errors after initial login
- Identified incorrect role-to-user mappings
- Corrected `roles_mapping.yml` and reapplied configuration using `securityadmin.sh`
- Verified successful admin access to Wazuh dashboard

### Outcome
- Wazuh dashboard fully operational
- Ready to onboard agents and generate security events
- Outcome can be seen in the screenshot labeled "1- Wazuh SIEM Dashboard"

### Wazuh Dashboard Overview

Below is the initial Wazuh dashboard after successful deployment and configuration.  
At this stage, no agents have been enrolled yet.

![Wazuh Dashboard](screenshots/wazuh-dashboard-home.png)

--------------------------------

##  Agent Deployment & Connectivity (Windows 10)

A Windows 10 victim machine was successfully onboarded into the Wazuh SIEM using the Wazuh agent. This step enables endpoint visibility, log collection, and alert generation from the monitored host.

###  Deployment Process
- Installed the Wazuh agent on the Windows 10 VM
- Retrieved the Wazuh Manager IP address from the Linux manager (`ip a`)
- Generated an authentication key using the `manage_agents` utility on the Wazuh Manager
- Imported the authentication key into the Windows Wazuh Agent Manager
- Started the Wazuh agent service on the Windows host
- Verified agent connectivity and status via the Wazuh dashboard

###  Issues Encountered & Resolutions

**Issue 1: Agent showed as “Never connected”**
- Cause: An agent entry was created before the Windows agent service was fully configured
- Resolution:
  - Generated a new authentication key
  - Re-imported the key into the Windows agent
  - Restarted the Wazuh agent service

**Issue 2: Duplicate agent entry appeared**
- Cause: Multiple agent registrations during initial setup
- Resolution:
  - Identified the inactive agent in the Wazuh dashboard
  - Removed unused entries using the Wazuh manager CLI
  - Confirmed only one active agent remained

**Issue 3: Windows agent manager not found via Run command**
- Cause: PATH shortcut not available
- Resolution:
  - Manually navigated to the installation directory:
    `C:\Program Files (x86)\ossec-agent\`
  - Launched the agent manager executable directly

###  Result
- The Windows 10 agent successfully connected to the Wazuh Manager
- Agent status shows **Active**
- Logs are now being forwarded and monitored in real time

![Windows 10 Agent Connected](screenshots/windows10-agent-connected.png)

--------------------------------

## Generating my first Wazuh alert (Windows 10 failed logon)

### Goal
Generate and confirm a real Wazuh alert from a Windows 10 endpoint by creating multiple failed login attempts (Event ID **4625**) and verifying they appear in the Wazuh **Security events** dashboard.

### What I did
1. Confirmed the Windows agent was connected and reporting.
2. Created multiple failed password attempts on the Windows 10 VM to trigger **4625** (logon failure).
3. Opened Wazuh Dashboard → **Security events** and verified events were flowing from my Windows agent.
4. Filtered the results to confirm the events came from my Windows machine and matched the failed logon alert.

### Issues I ran into (and fixes)

**Problem 1: No events showing in Security events**
- I initially saw “No results for selected time range”.
- **Fix:** Expanded the time range (Last 24 hours / Last 7 days) and refreshed.

**Problem 2: Password failures didn’t appear even though I knew I generated them**
- The Windows machine showed failed logons, but Wazuh didn’t show them at first.
- **Fix:** Enabled the correct Windows audit policies for logon failures and confirmed Security log collection was enabled in the Wazuh agent configuration.

**Problem 3: Dashboard filters were confusing / seemed stuck**
- A filter like `manager.name: wazuh-VirtualBox` made it look like nothing was coming in.
- **Fix:** Removed/cleared filters and used a direct query filter on the Windows agent instead (e.g., `agent.name:"DESKTOP-9S1EKMG"`), then searched for the failed logon events.

**Problem 4: Authentication/permissions issues when testing the indexer**
- Some curl checks returned **Unauthorized** until credentials were corrected.
- **Fix:** Reset/updated the indexer admin password and updated the Wazuh dashboard config to match, then restarted the dashboard service.

### Proof / Result
- Windows 10 agent events are now visible in **Security events**
- Failed logon events (Event ID **4625**) are being ingested and can be filtered/searched
- Alerts now show with current timestamps and correct agent name

![Wazuh 4625 Alert Visible](screenshots/wazuh-4625-alert.png)


