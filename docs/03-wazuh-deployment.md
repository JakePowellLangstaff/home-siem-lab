# 03 — Docker and Wazuh deployment

## Objective

Deploy a repeatable, single-node Wazuh platform that centralises endpoint telemetry and supports alert investigation through a web dashboard.

## Docker installation and validation

Docker Engine and the Docker Compose plugin were installed from Docker's Ubuntu repository. The daemon and container runtime were enabled automatically.

The installation was validated with:

```bash
sudo docker run hello-world
```

This confirmed that the client could contact the daemon, pull an image, create a container and return its output.

## Wazuh repository layout

The official Wazuh Docker deployment was placed under:

```text
/opt/stacks/wazuh-docker/single-node
```

The single-node deployment contains three principal services:

| Service | Purpose |
|---|---|
| Wazuh manager | Agent enrollment, event decoding, rules and alert generation |
| Wazuh indexer | Searchable security-event and alert storage |
| Wazuh dashboard | Web interface for endpoint visibility and threat hunting |

TLS materials were generated before the stack was started. Certificate presence was checked in the Wazuh certificate configuration directory.

## Deployment

From the `single-node` directory:

```bash
sudo docker compose up -d
sudo docker compose ps
```

All three Wazuh 4.14.7 containers started successfully.

## Published services

| Port | Protocol | Purpose |
|---:|---|---|
| 443 | TCP | Wazuh dashboard HTTPS |
| 1514 | TCP | Agent event communication |
| 1515 | TCP | Agent enrollment |
| 514 | UDP | Optional syslog ingestion |
| 55000 | TCP | Wazuh API |
| 9200 | TCP | Indexer API |

These ports are not intentionally forwarded from the public internet. Future hardening will restrict host firewall access to required LAN and Tailscale sources.

## Credential hardening

The deployment's documented default credentials were replaced before endpoint onboarding. Three separate credential roles were identified and handled independently:

| Account | Function |
|---|---|
| `admin` | Human dashboard administrator |
| `wazuh-wui` | Internal dashboard-to-manager API service account |
| `kibanaserver` | Internal dashboard-to-indexer service account |

The dashboard administrator password, internal API password and indexer service password were changed and synchronised across the necessary configuration files. The indexer's security configuration was reapplied and dashboard login was retested.

No credential values or hashes are stored in this repository.

## Operational commands

```bash
cd /opt/stacks/wazuh-docker/single-node
sudo docker compose ps
sudo docker compose logs --tail=100
sudo docker compose up -d
sudo docker compose down
```

## Result

The dashboard became available over HTTPS, all core containers remained running, and the platform was ready to accept endpoints.

