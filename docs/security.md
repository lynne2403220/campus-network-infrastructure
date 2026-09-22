# Security Design

## Layer 1 — Perimeter Security

### Site-to-Site VPN

- Encrypted tunnel between **ISP** and **MIU_Branch 1**
- Protects all branch traffic traversing the public internet
- Uses IPsec for confidentiality and integrity
- Public endpoints: `64.100.1.1/27` (ISP) and `64.100.1.2/27` (Branch_GW)

### NAT / PAT

- Configured on **MIU-GW**
- Translates private campus addresses to the public IP `209.165.200.226`
- Allows outbound internet access from all VLANs
- Inbound traffic restricted

## Layer 2 — Access Security

### Port Security

- Enabled on access switch ports
- Limits MAC addresses per port
- Prevents MAC flooding attacks

### VLAN Segmentation

- Each building has its own VLANs
- Inter-VLAN traffic routed through MLS
- No direct Layer 2 connectivity between buildings

## Layer 3 — Traffic Filtering

### Extended ACLs

- Applied on MLS devices and MIU-GW
- Control inter-VLAN traffic
- Restrict access to sensitive services (server farm)

### Example policies:

| Source | Destination | Action |
|--------|-------------|--------|
| Student VLANs | Admin VLANs | Deny |
| Any VLAN | DNS Server | Permit |
| Branch VLANs | Server Farm | Permit (via VPN) |
| Public Internet | Internal Servers | Deny (except published web) |

## Access Control

- SSH-only device management
- Enable secret passwords on all routers and switches
- Console and VTY line passwords
- Login banners on all devices

## Summary

| Layer | Control |
|-------|---------|
| Perimeter | Site-to-Site VPN, NAT/PAT |
| Access | Port Security, VLANs |
| Traffic | Extended ACLs |
| Management | SSH, passwords, banners |

