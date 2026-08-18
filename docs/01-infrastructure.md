# 01 — Infrastructure and baseline

## Objective

Repurpose an HP ProLiant server as a stable, remotely managed security lab capable of hosting a containerised SIEM and multiple supporting services.

## Hardware identified

| Component | Details |
|---|---|
| Server | HP ProLiant DL360p Gen8 |
| Processor | Intel Xeon E5-2643 at 3.30 GHz |
| Compute | 4 physical cores, 8 logical processors |
| Memory | 48 GB ECC RAM |
| Storage controller | HP Smart Array P420i |
| Logical storage | Approximately 558 GB |
| Network | Four onboard Ethernet interfaces; active interface `eno4` |

The hardware is more than sufficient for a single-node Wazuh lab. The main constraint is storage resilience rather than compute or memory.

## Operating system installation

Ubuntu Server 24.04 LTS was installed from a bootable USB device. OpenSSH was selected to support headless administration.

The server was assigned:

- Hostname: `homeserver`
- Time synchronisation: active through NTP
- System timezone: UTC, suitable for consistent security-event correlation

## Network baseline

The active interface was identified with:

```bash
ip link
ip -4 addr show eno4
```

The Vodafone router initially assigned the server a dynamic LAN address. A DHCP reservation was then created using the active interface's MAC address, providing a stable address for endpoint enrollment and administration.

Using a router-side reservation avoids manually hard-coding network settings on the server while keeping the service address predictable.

## Host validation

```bash
nproc
free -h
df -h /
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
timedatectl
```

These commands established the available CPU, memory, filesystem layout and clock status before deploying workload services.

## LVM storage correction

Ubuntu initially allocated only 100 GB of the approximately 557 GB LVM volume group to the root logical volume. The unused capacity was confirmed with:

```bash
sudo vgs
sudo lvs
```

The root logical volume and ext4 filesystem were expanded online:

```bash
sudo lvextend -l +100%FREE -r /dev/mapper/ubuntu--vg-ubuntu--lv
```

Result:

- Root filesystem increased from approximately 98 GB usable to approximately 548 GB
- No unallocated extents remained in the volume group
- Expansion completed without taking the server offline

## Kernel prerequisite

The Wazuh indexer requires an adequate `vm.max_map_count`. The current value was checked:

```bash
sysctl vm.max_map_count
```

The server returned `1048576`, exceeding Wazuh's minimum requirement, so no change was necessary.

## Risk identified: RAID 0

The two physical drives form a RAID 0 logical disk. RAID 0 increases usable capacity but provides no fault tolerance. Failure of either disk results in total array loss.

Planned mitigation:

- Keep deployment configuration in version control
- Create scheduled off-host backups
- Export important investigation evidence
- Consider rebuilding storage with redundancy when suitable drives are available

