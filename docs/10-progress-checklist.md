# 10 - Project progress checklist

## Completed

- [x] Install Ubuntu Server 24.04 and expand LVM storage
- [x] Configure UFW, OpenSSH and Tailscale remote administration
- [x] Install Docker Engine and Docker Compose
- [x] Deploy and harden Wazuh 4.14.7 single-node services
- [x] Enrol and validate Windows endpoint `win-lab-01`
- [x] Collect Sysmon process and network events
- [x] Create and trigger custom Wazuh rule `100100`
- [x] Validate built-in encoded PowerShell coverage
- [x] Configure FIM creation, modification and deletion monitoring
- [x] Enable Who-data user and process attribution
- [x] Investigate Windows failed-logon Event ID `4625`
- [x] Trigger multiple-logon correlation rule `60204`
- [x] Establish a 29% CIS Windows 10 SCA baseline
- [x] Remediate account-lockout threshold, duration and reset window

## In progress

- [ ] Verify the post-remediation SCA scan and score change

## Planned

- [ ] Review vulnerability-detection findings
- [ ] Add and validate Windows Defender telemetry
- [ ] Save analyst searches and write an investigation report
- [ ] Implement Wazuh retention and disk monitoring
- [ ] Create and test off-host backups
- [ ] Restrict exposed Wazuh ports to required LAN/Tailscale sources
- [ ] Add Suricata network monitoring
- [ ] Enrol an additional Linux endpoint
