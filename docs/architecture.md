# Network Architecture

## Overview

The MIU Campus Network follows a **3-tier hierarchical design** combined with a **centralized service model** and **multi-site connectivity**:

- **Core / Gateway** — MIU-GW (campus edge), ISP (external)
- **Distribution** — Multi-Layer Switches (MLS) per building
- **Access** — Dual access switches per building (redundant)
- **Services** — Centralized server farm
- **Remote Sites** — Branch office + home network

## Device Inventory

### Core / WAN Layer

| Device | Role | Interface / IP |
|--------|------|----------------|
| ISP Router | External connectivity, public IP allocation | `209.165.200.225/28`, `64.100.1.1/27`, `64.100.2.1/27` |
| MIU-GW | Campus edge router, NAT, VPN endpoint | `209.165.200.226/28` |

### Server Farm

Connected behind `SW-S` on a dedicated VLAN:

| Server | Service |
|--------|---------|
| DHCP Server | Centralized IP allocation for all VLANs |
| Email Server | Internal mail |
| Web Server | `miu.edu.eg` |
| DNS Server | Domain name resolution |
| NTP / Syslog Server | Time sync + central logging |

### Campus Buildings

| Building | MLS | Access Switches | Routing Protocol | VLANs |
|----------|-----|-----------------|------------------|-------|
| Main | Main-MLS | Main-SW1, Main-SW2 | OSPF | 100, 200 |
| S | S-MLS | S-SW1, S-SW2 | OSPF | 25, 400 |
| N | N-MLS | N-SW1, N-SW2 | EIGRP | 500, 600 |
| R | R-MLS | R-SW1, R-SW2 | EIGRP | 700, 800 |

Each building uses:
- A **triangle topology** between MLS and the two access switches
- **3 LACP EtherChannel bundles** (Ch1, Ch2, Ch3) for link redundancy
- **Per-building VLAN segmentation** for department isolation

### Branch Office — MIU_Branch 1

| Device | Role |
|--------|------|
| Branch_GW | Edge router, VPN endpoint (`64.100.1.2/27`) |
| B_S1, B_S2 | Access switches with LACP EtherChannel |
| PC11, PC12 | End hosts |

- VLAN 2 — `192.168.2.0/24`
- VLAN 3 — `192.168.3.0/24`
- Connected to the ISP via **Site-to-Site VPN**

### Home Network

| Device | IP |
|--------|-----|
| Wireless Home Router (public) | `64.100.2.2/27` |
| LAN subnet | `192.168.10.0/25` |
| Laptop | `192.168.10.10/25` |
| Smartphone | `192.168.10.20/25` |
| Tablet | `192.168.10.30/25` |

## Design Principles

1. **Hierarchy** — Separate core, distribution, and access layers
2. **Redundancy** — LACP EtherChannels prevent single points of failure
3. **Segmentation** — VLANs isolate traffic by building/function
4. **Centralization** — All services hosted in one server farm
5. **Scalability** — New buildings can be added with the same pattern
6. **Security** — VPN, ACLs, NAT, and port security at every layer
