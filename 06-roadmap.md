# 04 — Windows endpoint and Sysmon integration

## Objective

Enroll a Windows endpoint, verify secure agent communications and enrich its telemetry with Microsoft Sysmon.

## Endpoint enrollment

A Windows 10 Pro lab workstation was enrolled as:

```text
win-lab-01
```

Before installation, connectivity to the Wazuh manager was tested from PowerShell:

```powershell
Test-NetConnection <WAZUH_SERVER_IP> -Port 1515
Test-NetConnection <WAZUH_SERVER_IP> -Port 1514
```

Both tests succeeded. The dashboard-generated Wazuh Agent 4.14.7 deployment command was then run with administrator privileges.

The endpoint reported as active in the dashboard and was independently confirmed on the manager:

```bash
sudo docker exec single-node-wazuh.manager-1 \
  /var/ossec/bin/agent_control -lc
```

Expected state:

```text
ID: 001, Name: win-lab-01, IP: any, Active
```

`IP: any` allows the enrolled endpoint's address to change without invalidating its agent identity.

## Existing Sysmon assessment

Sysmon was already installed on the endpoint. Its service, path and startup state were checked:

```powershell
Get-Service -Name "Sysmon*"

Get-CimInstance Win32_Service |
  Where-Object Name -Like "Sysmon*" |
  Select-Object Name, State, StartMode, PathName
```

The `Sysmon64` service was running automatically from `C:\Windows\Sysmon64.exe`.

Recent Event IDs 1 and 5 confirmed that Sysmon was operational. Its active configuration showed SHA256 hashing but no rules and network monitoring disabled.

## Minimal Sysmon policy

A deliberately small XML policy was created to capture:

- Event ID 1 — process creation
- Event ID 3 — network connection
- SHA256 executable hashes
- Certificate revocation checking

The example is stored at [`config/sysmon-minimal.xml`](../config/sysmon-minimal.xml).

It was applied dynamically without reinstalling Sysmon:

```powershell
& "$env:windir\Sysmon64.exe" -c "C:\Sysmon\sysmonconfig.xml"
```

Network-event creation was confirmed using `Get-WinEvent` after making a test HTTPS connection.

## Centralised Wazuh collection

A dedicated Wazuh agent group named `windows-sysmon` was created. This avoids distributing Windows Event Channel settings to future Linux agents.

The group configuration is stored in this repository as [`config/wazuh-windows-sysmon-agent.conf`](../config/wazuh-windows-sysmon-agent.conf).

The policy tells agents to analyse:

```text
Microsoft-Windows-Sysmon/Operational
```

Configuration delivery was verified from the manager:

```bash
sudo docker exec single-node-wazuh.manager-1 \
  /var/ossec/bin/agent_groups -S -i 001
```

Result:

```text
Agent '001' is synchronized.
```

The Windows agent was restarted, remained healthy, and recorded:

```text
Analyzing event log: 'Microsoft-Windows-Sysmon/Operational'.
```

## Why start with only two Sysmon event types?

Sysmon can generate substantial event volume. Starting with process creation and network connection telemetry provides immediate investigative value while allowing storage and noise to be measured before adding DNS, registry, file creation, image loading or process-access events.

