# 05 — Validation and initial findings

## Validation strategy

The integration was tested layer by layer:

1. Confirm the Windows endpoint can reach enrollment and event ports.
2. Confirm the Wazuh agent is enrolled and active.
3. Confirm Sysmon is running and writing to its Operational event channel.
4. Apply a minimal policy and confirm Event ID 3 network records.
5. Confirm the Wazuh group configuration is synchronized.
6. Confirm the agent is analysing the Sysmon channel.
7. Generate benign process and network activity.
8. Investigate resulting alerts in Wazuh Threat Hunting.

## Benign test activity

```powershell
whoami /all
Test-NetConnection example.com -Port 443
```

These commands were selected because they safely generate process and network telemetry without introducing malware or exploit tooling.

## Initial alert set

An exported sample contained 48 alerts from `win-lab-01` over approximately two minutes:

| Count | Rule ID | Description | Level |
|---:|---:|---|---:|
| 18 | 92066 | `SecEdit.exe` launched by PowerShell from a location considered suspicious by the rule | 4 |
| 18 | 92021 | PowerShell used to delete files or directories | 3 |
| 10 | 92031 | Discovery activity executed | 3 |
| 1 | 92036 | `net.exe` started through a Windows command shell | 3 |
| 1 | 92052 | Windows command prompt started by an abnormal process | 4 |

## Example investigation: account discovery

One Rule 92031 alert contained:

```text
Parent process: C:\Windows\SysWOW64\net.exe
Parent command: net user
Child process:  C:\Windows\SysWOW64\net1.exe
Child command:  net1 user
User:           NT AUTHORITY\SYSTEM
Working path:   C:\Program Files (x86)\ossec-agent\
Sysmon event:   1 (Process Create)
MITRE:          T1087 Account Discovery
```

## Analyst assessment

The rule correctly identified behaviour associated with account discovery. However, the context strongly suggests expected Wazuh assessment activity rather than an intrusion:

- The process ran as `SYSTEM`.
- Its working directory was the Wazuh agent installation directory.
- It occurred immediately after synchronising and restarting the agent.
- The alert severity was low.

This demonstrates why an alert is not automatically an incident. A SOC analyst must combine the detection rule with process ancestry, execution user, working directory, timing and expected administrative activity.

## Data-flow result

```text
Windows activity
  -> Sysmon
  -> Microsoft-Windows-Sysmon/Operational
  -> Wazuh agent
  -> Wazuh manager rules
  -> Wazuh indexer
  -> Threat Hunting dashboard
```

The end-to-end pipeline is operational.

## Evidence-handling note

Raw exports may include account names, email addresses, SIDs, hostnames, internal addresses, command lines and file hashes. Only redacted screenshots or small sanitised extracts should be committed publicly.

