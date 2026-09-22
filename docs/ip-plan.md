# IP Addressing Plan

## Public IP Allocation

| Range | Purpose |
|-------|---------|
| `209.165.200.224/28` | ISP uplink block |
| `209.165.200.225/28` | ISP external interface |
| `209.165.200.226/28` | MIU-GW public interface |
| `64.100.1.0/27` | Branch public block (VPN) |
| `64.100.1.1/27` | ISP side of VPN tunnel |
| `64.100.1.2/27` | Branch_GW public interface |
| `64.100.2.0/27` | Home network public block |
| `64.100.2.1/27` | ISP to home router |
| `64.100.2.2/27` | Home router public interface |

## Campus VLAN Plan

| Building | VLAN | Purpose | Hosts |
|----------|------|---------|-------|
| Main | 100 | Main Building — Zone A | 61 |
| Main | 200 | Main Building — Zone B | 30 |
| S | 25 | S Building — Zone A | 12 |
| S | 25 | S Building — Zone B | 20 |
| S | 400 | S Building — Zone C | 20 |
| N | 500 | N Building — Zone A | 15 |
| N | 600 | N Building — Zone B | 29 |
| R | 700 | R Building — Zone A | 31 |
| R | 800 | R Building — Zone B | 25 |

## Branch VLAN Plan

| VLAN | Subnet | Gateway | Hosts |
|------|--------|---------|-------|
| 2 | `192.168.2.0/24` | `192.168.2.1` | PC11 |
| 3 | `192.168.3.0/24` | `192.168.3.1` | PC12 |

## Home Network

| Device | IP |
|--------|-----|
| Home Router (LAN) | `192.168.10.1/25` |
| Laptop | `192.168.10.10/25` |
| Smartphone | `192.168.10.20/25` |
| Tablet | `192.168.10.30/25` |

## VLSM Considerations

Host counts per VLAN vary from 12 to 61. Subnets are sized to accommodate current hosts with headroom for growth. DHCP Relay on each MLS forwards requests from building VLANs to the centralized DHCP server.
