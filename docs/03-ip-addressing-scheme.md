# 03 — IP Addressing Scheme

## Addressing plan

The lab uses four classful Class A segments, one per router interface, each with a `/8` (255.0.0.0) mask. This mirrors the interface labels fixed to the devices on CRAW Rack 1.

### Router interfaces

| Device   | Interface        | IP Address | Subnet Mask | CIDR | Network      |
|----------|------------------|------------|-------------|------|--------------|
| Router 1 | FastEthernet 0/0 | `10.0.0.2` | `255.0.0.0` | `/8` | `10.0.0.0/8` |
| Router 1 | FastEthernet 0/1 | `30.0.0.1` | `255.0.0.0` | `/8` | `30.0.0.0/8` |
| Router 2 | FastEthernet 0/0 | `20.0.0.2` | `255.0.0.0` | `/8` | `20.0.0.0/8` |
| Router 2 | FastEthernet 0/1 | `40.0.0.1` | `255.0.0.0` | `/8` | `40.0.0.0/8` |

### Host / service plan (per segment)

| Segment      | Gateway    | DHCP pool range                 | Reserved (excluded)        | DNS server |
|--------------|------------|---------------------------------|----------------------------|------------|
| `10.0.0.0/8` | `10.0.0.2` | `10.0.0.10` – `10.0.0.254`      | `10.0.0.1` – `10.0.0.9`    | `10.0.0.5` |
| `20.0.0.0/8` | `20.0.0.2` | `20.0.0.10` – `20.0.0.254`      | `20.0.0.1` – `20.0.0.9`    | `10.0.0.5` |
| `30.0.0.0/8` | `30.0.0.1` | `30.0.0.10` – `30.0.0.254`      | `30.0.0.1` – `30.0.0.9`    | `10.0.0.5` |
| `40.0.0.0/8` | `40.0.0.1` | `40.0.0.10` – `40.0.0.254`      | `40.0.0.1` – `40.0.0.9`    | `10.0.0.5` |

> Infrastructure addresses (gateways, DNS, management) are excluded from the DHCP pools so they are never leased to clients.

## Addressing rationale

- **Classful `/8` segments** keep the lab simple and make each network instantly identifiable by its first octet (10 → LAN A, 20 → LAN B, 30 → LAN C, 40 → LAN D).
- **Low host addresses reserved** (`.1`–`.9`) for gateways, DNS, and management.
- **DHCP covers the usable range** so client PCs are configured automatically.

## Notes from the live session

During the initial bring-up (captured in `media/console/03-interface-ip-configuration.png`), the router first had overlapping addresses applied on both FastEthernet interfaces within the same `10.0.0.0/8` network, which IOS rejected:

```
% 10.0.0.0 overlaps with FastEthernet0/1
FastEthernet0/0: incorrect IP address assignment
```

This is a useful teaching point: **two interfaces on the same router cannot belong to the same IP network.** The final scheme (table above) places each interface in a distinct network, resolving the overlap. See [`05-verification-and-troubleshooting.md`](05-verification-and-troubleshooting.md) for the full analysis.

## Quick reference

```
LAN A : 10.0.0.0/8    GW 10.0.0.2   (R1 Fa0/0)
LAN B : 20.0.0.0/8    GW 20.0.0.2   (R2 Fa0/0)
LAN C : 30.0.0.0/8    GW 30.0.0.1   (R1 Fa0/1)
LAN D : 40.0.0.0/8    GW 40.0.0.1   (R2 Fa0/1)
```
