# Redundancy & High Availability

## EtherChannel Design

Every campus building uses a **triangle topology** with three LACP EtherChannel bundles:


- **Ch1** — Inter-switch link (SW1 ↔ SW2)
- **Ch2** — MLS ↔ SW1
- **Ch3** — MLS ↔ SW2

## Why LACP?

| Feature | Benefit |
|---------|---------|
| **Link aggregation** | Combines multiple physical links into one logical link |
| **Load balancing** | Traffic distributed across member links |
| **Redundancy** | If one link fails, traffic continues on remaining links |
| **Standard-based** | IEEE 802.3ad — vendor-neutral |

## Failure Scenarios

| Failure | Result |
|---------|--------|
| One physical link in a bundle fails | Bundle continues with remaining links |
| SW1 fails | Traffic reroutes via MLS → SW2 → access |
| MLS uplink fails | Inter-switch Ch1 keeps SW1/SW2 connected |

## Branch Redundancy

- `B_S1` and `B_S2` are connected via **Ch1 EtherChannel**
- Both connect to `Branch_GW`

## Server Farm

- Servers connect to `SW-S`, which dual-homes toward MIU-GW

