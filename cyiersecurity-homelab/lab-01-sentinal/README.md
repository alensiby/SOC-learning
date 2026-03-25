"# Lab 01 — Microsoft Sentinel SIEM: Data Connectors & Detection

## Overview
Deployed Microsoft Sentinel on Azure Log Analytics Workspace and connected
multiple data sources to build a functional SIEM environment. Configured
data connectors for Windows Security Events, and Azure Activity logs. 
Verified ingestion using KQL queries and
activated pre-built detection rules covering brute force, privilege
escalation, and audit log tampering.

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

## Key Technical Concepts Learned

### Log Ingestion Mechanisms
| Method | Used For | How It Works |
|--------|----------|--------------|
| Diagnostic Settings | Azure Activity | Azure-native pipeline, no agent |
| Azure Monitor Agent | Windows/Linux VMs | Agent reads Event Log, sends HTTPS |
| REST API / CEF | Third-party tools | Tools push logs to workspace |

### Key Windows Event IDs Monitored
| Event ID | Description | Why It Matters |
|----------|-------------|----------------|
| 4624 | Successful logon | Baseline for user activity |
| 4625 | Failed logon | Brute force and credential attacks |
| 1102 | Audit log cleared | Attacker covering tracks |

## Verification Queries
```kql
// Confirm Windows events are flowing
SecurityEvent
| where TimeGenerated > ago(1h)
| summarize Count = count() by EventID, Computer
| order by Count desc

// Confirm Entra ID login failures
SigninLogs
| where ResultType != 0
| project TimeGenerated, UserPrincipalName, ResultType, ResultDescription
| order by TimeGenerated desc
```

## Screenshots
- `screenshots/01-sentinel-overview.png` — Sentinel dashboard after data connections
- `screenshots/02-data-connectors-connected.png` — All connectors showing Connected
- `screenshots/03-kql-security-events.png` — KQL query returning SecurityEvent data
- `screenshots/04-analytics-rules-active.png` — Enabled analytics rules list
- `screenshots/05-first-incident.png` — First Sentinel incident created from failed logins

## Tools & Services Used
Microsoft Sentinel · Log Analytics Workspace · Azure Monitor Agent · Microsoft Entra ID
Azure Activity · Azure Policy · KQL (Kusto Query Language) · Windows Server

## Skills Demonstrated
SIEM deployment · Cloud log ingestion · Data connector configuration
KQL querying · Security event simulation · Detection rule activation
Azure identity security · Audit log analysis"<img width="464" height="1666" alt="image" src="https://github.com/user-attachments/assets/864c87bd-86c5-480a-9664-b1e5c95305eb" />
