Investigates a Cortex XDR incident containing internal port scan alerts. The playbook:
- Syncs data with Cortex XDR
- Enriches the hostname and IP address of the attacking endpoint
- Notifies management about host compromise
- Escalates the incident in case of lateral movement alert detection
- Hunts malware associated with the alerts across the organization
- Blocks detected malware associated with the incident
- Blocks IPs associated with the malware
- Isolates the attacking endpoint
- Allows manual blocking of ports that were used for host login following the port scan

## Dependencies
This playbook uses the following sub-playbooks, integrations, and scripts.

### Sub-playbooks
* IP Enrichment - Internal - Generic v2
* Isolate Endpoint - Generic
* Block IP - Generic v3
* PANW - Hunting and threat detection by indicator type V2
* Block File - Generic v2
* Calculate Severity - Generic v2

### Integrations
* CortexXDRIR

### Scripts
* StopScheduledTask
* AssignAnalystToIncident
* XDRSyncScript
* IsIPInRanges
* SetAndHandleEmpty

### Commands
* xdr-update-incident
* setIncident
* xdr-get-endpoints
* send-mail
* closeInvestigation

## Playbook Inputs
---

| **Name** | **Description** | **Default Value** | **Required** |
| --- | --- | --- | --- |
| WhitelistedPorts | A list of comma-separated ports that should not be blocked even if used in an attack. |  | Optional |
| BlockAttackerIP | Whether attacking IPs should be automatically blocked using firewalls. | False | Optional |
| WhitelistedHostnames | A list of comma-separated hostnames that should not be isolated even if used in an attack. | AdminPC | Optional |
| EmailAddressesToNotify | A list of comma-separated values of email addresses that should receive a notification about compromised hosts. |  | Optional |
| CriticalUsernames | A list of comma-separated names of critical users in the organization. This will affect the calculated severity of the incident. |  | Optional |
| IsolateEndpointAutomatically | Whether to automatically isolate endpoints, or opt for manual user approval. True means isolation will be done automatically. | False | Optional |
| InternalIPRanges | A list of IP ranges to check the IP against. The list should be provided in CIDR notation, separated by commas. An example of a list of ranges would be: "172.16.0.0/12,10.0.0.0/8,192.168.0.0/16" \(without quotes\). If a list is not provided, will use default list provided in the IsIPInRanges script \(the known IPv4 private address ranges\). |  | Optional |
| CriticalHostnames | A list of comma-separated names of critical endpoints in the organization. This will affect the calculated severity of the incident. | AdminPC | Optional |
| RoleForEscalation | The name of the Cortex XSOAR role of the users that the incident can be escalated to in case of developments like lateral movement. |  | Optional |
| BlockMaliciousFiles | Whether to automatically block malicious files involved with the incident across all endpoints in the organization. | False | Optional |
| CriticalADGroups | CSV of DN names of critical Active Directory groups. This will affect the severity calculated for this incident. | admins | Optional |
| OnCall | Set to true to assign only the users that are currently on shift. | false | Optional |

## Playbook Outputs
---
There are no outputs for this playbook.

## Playbook Image
---
![Cortex XDR - Port Scan](../doc_files/Cortex_XDR_-_Port_Scan.png)