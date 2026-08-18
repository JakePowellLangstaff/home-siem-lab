# 09 - Roadmap

This roadmap turns the working platform into a repeatable SOC and incident-response portfolio project.

## Phase 1 — Platform baseline — complete

- [x] Install Ubuntu Server
- [x] Configure stable LAN addressing
- [x] Enable and validate OpenSSH
- [x] Configure UFW
- [x] Establish remote administration through Tailscale
- [x] Install and validate Docker
- [x] Expand the root LVM filesystem
- [x] Deploy Wazuh single-node stack
- [x] Replace default Wazuh credentials
- [x] Enroll first Windows endpoint
- [x] Integrate Sysmon process and network telemetry
- [x] Validate events and MITRE mappings
- [x] Enable File Integrity Monitoring and Who-data
- [x] Validate failed-logon and brute-force correlation alerts
- [x] Establish a CIS Windows 10 SCA baseline

## Phase 2 — Detection engineering

- [x] Define and run safe PowerShell detection exercises
- [x] Record expected telemetry before running the exercises
- [x] Triage the generated alerts
- [x] Create and validate custom Wazuh rule `100100`
- [x] Identify and remove a redundant encoded-PowerShell custom rule
- [x] Document rule logic and MITRE mapping
- [ ] Add a reusable investigation template

## Phase 3 — Telemetry expansion

- [ ] Add Windows Defender Operational logs
- [ ] Review Wazuh vulnerability-detection findings
- [ ] Evaluate Sysmon DNS Query events
- [ ] Evaluate registry and file-creation events
- [ ] Tune exclusions based on measured noise
- [ ] Enroll an Ubuntu endpoint
- [ ] Create Windows and Linux agent groups

## Phase 4 — Reliability and hardening

- [ ] Configure off-host backups
- [ ] Test configuration restoration
- [ ] Define index retention targets
- [ ] Monitor disk usage and container health
- [ ] Restrict Wazuh ports with source-specific UFW rules
- [ ] Replace the dashboard's untrusted certificate
- [ ] Move SSH to key-only authentication after recovery testing
- [ ] Review Tailscale ACLs and device-key policy

## Phase 5 — Portfolio deliverables

- [ ] Add a final architecture diagram
- [ ] Add sanitised dashboard screenshots
- [ ] Write one complete incident-investigation case study
- [ ] Create an alert-triage runbook
- [ ] Summarise lessons learned and measurable outcomes
- [ ] Record a short demonstration video

## Immediate next actions

1. Verify the post-remediation SCA scan and record the score change.
2. Review vulnerability-detection findings and prioritise one remediation.
3. Add and validate Windows Defender telemetry.
4. Save analyst searches and write a concise investigation report.
5. Establish index retention, storage monitoring and off-host backups.
6. Add Suricata network monitoring and then onboard a Linux endpoint.
