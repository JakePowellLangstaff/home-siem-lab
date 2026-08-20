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
- [x] Enrol `win-admin-01` and the Ubuntu `homeserver` agent
- [x] Configure Sysmon telemetry on both Windows endpoints
- [x] Restrict externally published Wazuh services to required LAN bindings
- [x] Configure Tailscale Serve for tailnet-only dashboard access
- [x] Automate weekly, checksum-verified Wazuh backups
- [x] Deploy Grafana, Prometheus, Loki, Alloy, Node Exporter and cAdvisor
- [x] Validate six Prometheus scrape targets and Loki log ingestion
- [x] Import Ubuntu host and Docker container dashboards
- [x] Create a critical Prometheus target-availability alert
- [x] Deliver alert notifications to Discord
- [x] Validate firing and resolved states by stopping and restoring Node Exporter

## Planned

- [ ] Review vulnerability-detection findings
- [ ] Add and validate Windows Defender telemetry
- [ ] Save analyst searches and write an investigation report
- [ ] Finalise Wazuh, Prometheus and Loki retention targets
- [ ] Replicate verified backups off-host and test restoration
- [ ] Configure a public Grafana root URL for usable notification links
- [ ] Add Suricata network monitoring
