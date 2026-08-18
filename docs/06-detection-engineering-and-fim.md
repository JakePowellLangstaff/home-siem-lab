# 06 - Detection engineering and File Integrity Monitoring

## Objective

Develop and validate an environment-specific Wazuh rule, then extend endpoint monitoring to record file changes with user and process attribution.

## Custom detection rule

A benign marker file was opened from PowerShell:

```powershell
Start-Process -FilePath "notepad.exe" `
  -ArgumentList "C:\Lab\custom-rule-test.txt"
```

Sysmon recorded Event ID 1 with the image, complete command line, SHA-256 hash, user and PowerShell parent process. Wazuh's `sysmon_event1` group was confirmed from the installed ruleset before rule `100100` was added to `local_rules.xml`.

The rule matches the distinctive marker in `win.eventdata.commandLine` and generates a level-7 alert. Configuration was validated with `wazuh-analysisd -t` before restarting the manager. The resulting alert proved the full path from endpoint activity to a custom SIEM detection.

## Existing coverage assessment

A safe Base64-encoded PowerShell command was executed to test a second detection idea. Wazuh's existing rule `92057` produced a level-12 alert mapped to MITRE ATT&CK `T1059.001 - PowerShell`. The proposed custom rule was removed because it duplicated stronger built-in coverage.

This demonstrates an important detection-engineering practice: search existing coverage before maintaining another rule.

## File Integrity Monitoring

The `windows-sysmon` group configuration was expanded to monitor `C:\Lab` with:

- Real-time detection of file creation, modification and deletion
- Before/after file hashes
- Text-file content differences through `report_changes`
- Who-data attribution through Windows auditing

Validation generated:

| Rule | Meaning | Result |
|---:|---|---|
| 554 | File added | Detected |
| 550 | Integrity checksum changed | Detected with content diff |
| 553 | File deleted | Detected |

Who-data enriched alerts with `audit.user.name`, `audit.process.id` and `audit.process.name`. A controlled modification was attributed to the lab user and `powershell.exe`, with the added line and changed SHA-256 value preserved in the alert.

## Security note

`report_changes` can copy file content into monitoring data. It is therefore limited to a non-sensitive lab directory and should not be enabled indiscriminately.
