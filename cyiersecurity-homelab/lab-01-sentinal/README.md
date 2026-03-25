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
- `screenshots/Data_Connectors.png` — All connectors showing Connected
- `screenshots/KQL-verification.png` — KQL query returning SecurityEvent data
- `screenshots/active_rules.png` — Enabled analytics rules list
- `screenshots/Incident-Multiple-login-failures.png` — First Sentinel incident created from failed logins

## Tools & Services Used
Microsoft Sentinel · Log Analytics Workspace · Azure Monitor Agent · Microsoft Entra ID
Azure Activity · Azure Policy · KQL (Kusto Query Language) · Windows Server

## Skills Demonstrated
SIEM deployment · Cloud log ingestion · Data connector configuration
KQL querying · Security event simulation · Detection rule activation
Azure identity security · Audit log analysis
