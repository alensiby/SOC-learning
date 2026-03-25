# Lab 01 — Microsoft Sentinel SIEM: Data Connectors & Detection

## Overview
Deployed Microsoft Sentinel on Azure Log Analytics Workspace and connected
multiple data sources to build a functional SIEM environment. Configured
data connectors for Windows Security Events, and Azure Activity logs. 
Verified ingestion using KQL queries and
activated pre-built detection rules covering brute force, and audit log tampering.

## Architecture
```
  Azure VM (Windows Server)
       │ Azure Monitor Agent (AMA)
       │ HTTPS port 443
       ▼
  Log Analytics Workspace ◄── Azure Activity Logs (Azure Policy)
       │                  
       ▼
  Microsoft Sentinel
  ├── Analytics Rules (detection engine)
  ├── Incidents (investigation queue)
  └── Automation (SOAR — covered in Lab 04)
```

## What I Built
- Log Analytics Workspace in Australia East (data sovereignty)
- Microsoft Sentinel deployed on the workspace
- Azure VM (Windows Server) as the simulated enterprise endpoint
- AMA configured via Data Collection Rule to forward Security Event logs
- Azure Activity connector via Azure Policy for subscription-wide coverage
- 2x pre-built analytics rules enabled and verified
