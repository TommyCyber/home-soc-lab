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

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

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
