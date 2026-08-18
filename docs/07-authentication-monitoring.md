# 07 - Windows authentication monitoring

## Objective

Validate individual failed-logon detection and Wazuh's higher-level correlation of repeated authentication failures.

## Safe simulation

A network logon was attempted against localhost using a nonexistent account:

```powershell
net use \\127.0.0.1\IPC$ /user:WazuhFakeUser DefinitelyWrongPassword
```

Windows generated Security Event ID `4625`. Wazuh classified it with rule `60122`, level 5.

## Investigation findings

| Field | Observed value | Interpretation |
|---|---|---|
| Target username | `WazuhFakeUser` | Nonexistent test identity |
| Source address | `127.0.0.1` | Locally generated test |
| Logon type | `3` | Network logon |
| Authentication package | `NTLM` | NTLM authentication path |
| Status | `0xC000006D` | Invalid credentials |
| Substatus | `0xC0000064` | User does not exist |

## Correlation test

Ruleset inspection established that Windows correlation rule `60204` uses a threshold of eight authentication failures within 240 seconds from the same IP address. Eight controlled failures triggered:

- Rule `60204`
- Level `10`
- Description: `Multiple Windows Logon Failures`
- MITRE ATT&CK `T1110 - Brute Force`

The test used a nonexistent username to avoid locking a real user account.
