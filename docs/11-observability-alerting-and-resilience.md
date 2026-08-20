# 11 - Observability, alerting and resilience

## Objective

Add infrastructure visibility alongside Wazuh so availability, resource pressure and container failures can be investigated with metrics and logs.

## Observability stack

A separate Docker Compose project was deployed under `/opt/stacks/observability`:

| Component | Purpose |
|---|---|
| Grafana | Dashboards, exploration and alerting |
| Prometheus | Time-series metrics and alert queries |
| Loki | Centralised log storage and search |
| Grafana Alloy | Journald and Docker log collection |
| Node Exporter | Ubuntu host metrics |
| cAdvisor | Docker container metrics |

Prometheus and Loki were provisioned as Grafana data sources. Six scrape targets reported `up=1`, covering Prometheus, Grafana, Loki, Alloy, Node Exporter and cAdvisor.

## Dashboards and investigation

- Imported an Ubuntu host dashboard for CPU, memory, swap, filesystem, network and uptime monitoring.
- Imported a Docker dashboard for per-container CPU, memory, network, block I/O and filesystem visibility.
- Validated Loki queries for systemd journal and Docker container logs.
- Kept observability services on the internal Compose network; only Grafana is published to the LAN.

## Alert lifecycle validation

A Grafana-managed alert named `Prometheus Target Unavailable` monitors the Prometheus `up` metric. It includes environment, instance, job and severity labels and sends notifications to a Discord contact point.

The complete lifecycle was tested safely:

1. Stopped only the Node Exporter container.
2. Confirmed `up=0` for `node-exporter:9100`.
3. Received a critical `Firing` notification after the pending period.
4. Restarted Node Exporter and confirmed it returned healthy.
5. Received the corresponding `Resolved` notification.

This validates collection, rule evaluation, alert routing, external notification and recovery detection.

## Backup and access improvements

- Automated weekly Wazuh backups with a systemd timer.
- Archived persistent Docker volumes and stack configuration.
- Generated and verified SHA-256 checksums for each backup archive.
- Confirmed Wazuh services restarted after the backup procedure.
- Restricted Wazuh service publishing to the required LAN address and removed unnecessary indexer/API exposure.
- Configured Tailscale Serve for encrypted, tailnet-only Wazuh dashboard access.

## Remaining work

- Copy backups to independent off-host storage.
- Perform and document a full restoration test.
- Finalise retention targets for Wazuh indices, Prometheus metrics and Loki logs.
- Set Grafana's public root URL so links in notifications open remotely.
