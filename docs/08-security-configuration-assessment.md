# 08 - Security Configuration Assessment

## Objective

Measure the Windows endpoint against a recognised hardening baseline, investigate one failed control and apply a risk-aware remediation.

## Baseline

Wazuh assessed the endpoint against **CIS Microsoft Windows 10 Enterprise Benchmark v4.0.0**:

| Result | Count |
|---|---:|
| Passed | 124 |
| Failed | 296 |
| Not applicable | 4 |
| Total | 424 |
| Score | 29% |

The score is a configuration-gap baseline, not evidence of compromise. The endpoint runs Windows 10 Pro, so some Enterprise-oriented controls may require contextual review.

## Investigated control

Control `15507` required an account-lockout threshold of five or fewer invalid attempts, but not zero. A read-only policy export showed:

```text
LockoutBadCount = 0
```

This disabled account lockout and increased exposure to repeated password guessing.

## Remediation

After considering accidental and malicious lockout risk, the local workstation policy was changed to:

```text
Lockout threshold: 5 attempts
Lockout duration: 15 minutes
Observation/reset window: 15 minutes
```

The setting was verified with `net accounts`. A follow-up Wazuh SCA scan is the next validation step and should reassess controls `15506`, `15507` and `15508`.

## Analyst lesson

CIS recommendations should not be applied blindly. Each finding requires assessment of platform edition, business purpose, availability risk and recovery options before remediation.
