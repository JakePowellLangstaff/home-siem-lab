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
- [x] Enrol a second Windows endpoint and the Ubuntu host
- [x] Configure Sysmon collection on both Windows endpoints

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
- [x] Enroll an Ubuntu endpoint
- [x] Create a central Windows Sysmon agent group

## Phase 4 — Reliability and hardening

- [x] Automate local Wazuh backups with a systemd timer
- [x] Verify backup archives with SHA-256 checksums
- [x] Restrict exposed Wazuh Docker ports to required LAN interfaces
- [x] Provide encrypted tailnet-only dashboard access with Tailscale Serve
- [x] Deploy Grafana, Prometheus, Loki, Alloy, Node Exporter and cAdvisor
- [x] Validate host, container and log data sources
- [x] Configure and test Discord alert delivery and recovery
- [ ] Replicate backups off-host
- [ ] Perform a full restoration test
- [ ] Define index retention targets
- [x] Monitor host resources and container health through Grafana
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

1. Review vulnerability-detection findings and prioritise one remediation.
2. Add and validate Windows Defender telemetry.
3. Save analyst searches and write a concise investigation report.
4. Replicate backups off-host and perform a documented restoration test.
5. Finalise Wazuh, Prometheus and Loki retention targets.
6. Add Suricata network monitoring.
