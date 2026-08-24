# Modular SOC Simulation Lab

Goal of Project: A progressively built cybersecurity home lab designed to simulate the practical Security Operations Center (SOC):

## Current Project Status

**Current Phase: Phase 0 - Lab Foundation** **Status: In Progress**

## Current Lab Architecture

```text
                         Windows Host
                    24 GB RAM / VMware
                              │
                              │
                         VMnet19
                       Host-only
                    192.168.50.0/24
                              │
                              │
                       ┌──────┴──────┐
                       │             │
                       ▼             ▼
                SOC-SPLUNK-01    Future VMs
                 192.168.50.10
                       │
                       │
                Ubuntu Server
                    24.04.4
                       │
                       ▼
                Splunk Enterprise
                  [Planned]
```

The initial architecture will eventually contain:

```text
                    Isolated SOC Network
                     192.168.50.0/24
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
      SOC-SPLUNK-01     SOC-WIN-01      SOC-KALI-01
       192.168.50.10    192.168.50.20    192.168.50.30
          SIEM            Endpoint         Attacker
```

## Phase 0 Network Design

### VMware Network

| Setting         | Configuration           |
| --------------- | ----------------------- |
| VMware Network  | `VMnet19`               |
| Network Type    | Host-only               |
| Subnet          | `192.168.50.0/24`       |
| Subnet Mask     | `255.255.255.0`         |
| Host Adapter    | `192.168.50.1`          |
| DHCP            | Disabled                |
| Default Gateway | None                    |
| Internet Access | None on the SOC network |

### IP Address Plan

| Device              | IP Address      | Role              | Status     |
| ------------------- | --------------- | ----------------- | ---------- |
| VMware Host Adapter | `192.168.50.1`  | Host management   | Configured |
| `SOC-SPLUNK-01`     | `192.168.50.10` | SIEM              | Configured |
| `SOC-WIN-01`        | `192.168.50.20` | Windows endpoint  | Planned    |
| `SOC-KALI-01`       | `192.168.50.30` | Attack simulation | Planned    |

The SOC network is intentionally separate from the normal home network.

---

# Phase 0 Progress

```text
Phase 0: Lab Foundation

[████████░░░░░░░░░░░░] In Progress
```

### Phase 0 Checklist

* [x] VMware network topology designed
* [x] Dedicated SOC network created
* [x] IP addressing scheme finalized
* [x] VM naming convention finalized
* [x] Resource allocation finalized
* [x] `SOC-SPLUNK-01` created
* [x] Ubuntu Server installed
* [x] Static network configuration completed
* [x] SSH configured
* [x] Host-to-VM connectivity tested
* [x] Network isolation validated
* [ ] `SOC-WIN-01` created
* [ ] `SOC-KALI-01` created
* [ ] Windows connectivity tested
* [ ] Kali connectivity tested
* [ ] Clean VM snapshots created
* [ ] Final Phase 0 network diagram created
* [ ] Final Phase 0 architecture documentation completed

---

# Resource Allocation

Initial VM allocation:

| VM              |       RAM |         CPU |       Disk |
| --------------- | --------: | ----------: | ---------: |
| `SOC-SPLUNK-01` |      8 GB |      4 vCPU |     100 GB |
| `SOC-WIN-01`    |      6 GB |      4 vCPU |      80 GB |
| `SOC-KALI-01`   |      2 GB |      2 vCPU |      40 GB |
| **Total**       | **16 GB** | **10 vCPU** | **220 GB** |

Approximately 8 GB of physical RAM is intentionally left available for the Windows host, VMware, and other background processes.

---

# Security and Isolation Model

The lab is designed to prevent controlled attack activity from reaching the physical home network.

```text
Home Network
xxx.xxx.x.x/24
       │
       X
       │
       │ No SOC VM connection
       │
       ▼
VMware VMnet19
192.168.50.0/24
       │
       ├── Splunk
       ├── Windows
       └── Kali
```

The SOC network currently has:

* No default gateway
* No Internet route
* DHCP disabled
* Host-only connectivity
* No bridged SOC adapters

Internet access, when genuinely required for installation or updates, will be handled separately and deliberately rather than being part of the normal attack-simulation network.

---


