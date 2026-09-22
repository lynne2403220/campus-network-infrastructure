# Routing Design

## Interior Routing Protocols

The campus uses a **mixed protocol design** to demonstrate real-world integration scenarios:

| Building | Protocol | Reason |
|----------|----------|--------|
| Main | OSPF | Open standard, scalable |
| S | OSPF | Consistent with Main |
| N | EIGRP | Cisco-native, fast convergence |
| R | EIGRP | Consistent with N |

## Why Mixed Protocols?

In real enterprise environments, networks often merge after acquisitions, department restructuring, or vendor-driven designs. This project simulates that scenario:
- **OSPF islands** (Main + S) represent one administrative domain
- **EIGRP islands** (N + R) represent another
- **Redistribution** at the MIU-GW allows end-to-end reachability

## OSPF Configuration Highlights

- **Area 0 (backbone)** used across Main and S buildings
- Router IDs assigned per MLS
- Passive interfaces on access-facing VLANs
- Default route advertised toward MIU-GW

## EIGRP Configuration Highlights

- **Autonomous System 10** used across N and R buildings
- Summary routes advertised between MLS devices
- Default route propagated toward MIU-GW

## Route Redistribution

Redistribution occurs at **MIU-GW**:

- OSPF → EIGRP: campus OSPF routes injected into EIGRP domain
- EIGRP → OSPF: N/R building routes injected into OSPF domain
- Static default route toward ISP redistributed into both

This provides **full reachability** between all four buildings plus the server farm.

## Edge Routing

- **MIU-GW** has a static default route to the ISP
- **ISP router** routes public blocks (`209.165.200.224/28`, `64.100.1.0/27`, `64.100.2.0/27`)
- **Branch_GW** routes branch VLANs via VPN tunnel to MIU-GW
- **Home Router** performs NAT for the `192.168.10.0/25` LAN

