# 05 — Verification & Troubleshooting

This document lists the commands used to verify the configuration and the troubleshooting workflow applied during the lab, including issues that were hit and resolved live.

## Verification commands

### Interfaces & addressing

```cisco
Router# show ip interface brief
Router# show interfaces FastEthernet0/0
Router# show running-config interface FastEthernet0/0
```

Example output captured during the session (`media/console/03-interface-ip-configuration.png`):

```
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        10.0.0.1        YES manual administratively down down
FastEthernet0/1        10.0.0.2        YES manual up                    up
```

> `administratively down` means the interface still needs `no shutdown`; `up/up` confirms both the line protocol and physical layer are good.

### Routing

```cisco
Router# show ip route
Router# show ip protocols
```

### Switching (L2)

```cisco
Switch# show vlan brief
Switch# show interfaces trunk
Switch# show interfaces status
Switch# show mac address-table
```

### Services

```cisco
Router# show ip dhcp binding        ! leases handed out
Router# show ip dhcp pool           ! pool utilization
Router# show hosts                  ! DNS / host table
```

### Secure access

```cisco
Router# show ip ssh                 ! SSH version & status
Router# show ssh                    ! active SSH sessions
Router# show users                  ! connected VTY/console users
```

## Layer 2 / Layer 3 analysis tools

### ARP (Address Resolution Protocol)

ARP maps Layer 3 IP addresses to Layer 2 MAC addresses. Inspecting the ARP table confirms that devices have resolved each other at the data-link layer:

```cisco
Router# show arp
Router# show ip arp
```

```
Protocol  Address     Age (min)  Hardware Addr   Type   Interface
Internet  10.0.0.2          0    0012.3f4a.1b2c  ARPA   FastEthernet0/0
Internet  10.0.0.10         3    00e0.1c7d.55a0  ARPA   FastEthernet0/0
```

On a PC the same table is viewed with `arp -a`.

### ICMP (ping) connectivity testing

ICMP verifies reachability and round-trip behavior between hosts and gateways:

```cisco
Router# ping 10.0.0.10
Router# ping 20.0.0.2              ! test cross-segment reachability
```

```
Sending 5, 100-byte ICMP Echos to 10.0.0.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
```

Reading the result codes:

| Symbol | Meaning |
|--------|---------|
| `!` | Echo reply received (success) |
| `.` | Timed out (no reply) |
| `U` | Destination unreachable |

## Troubleshooting log (issues hit & resolved)

### Issue 1 — Interface administratively down

**Symptom:** `show ip interface brief` showed `administratively down`.
**Cause:** Interfaces are shut by default.
**Fix:**

```cisco
Router(config)# interface FastEthernet0/1
Router(config-if)# no shutdown
```

Confirmed by the log message:

```
%LINK-3-UPDOWN: Interface FastEthernet0/1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up
```

### Issue 2 — Overlapping IP network

**Symptom:** When both router interfaces were addressed inside `10.0.0.0/8`, IOS rejected the second:

```
% 10.0.0.0 overlaps with FastEthernet0/1
FastEthernet0/0: incorrect IP address assignment
```

**Cause:** A router cannot have two interfaces in the same IP subnet.
**Fix:** Re-address each interface into a distinct network per the final plan (`10.0.0.0/8`, `20.0.0.0/8`, `30.0.0.0/8`, `40.0.0.0/8`). See [`03-ip-addressing-scheme.md`](03-ip-addressing-scheme.md).

### Issue 3 — Typo in interface name

**Symptom:**

```
Router(config)#interface FastThernet 0/1
                         ^
% Invalid input detected at '^' marker.
```

**Cause:** Misspelled keyword (`FastThernet`).
**Fix:** Use the correct keyword `interface FastEthernet 0/1` (or abbreviate `int fa0/1`). The `^` marker points at the first character IOS could not parse.

## Troubleshooting methodology

1. **Physical / Layer 1** — cabling, interface `up/up` state (`show ip interface brief`).
2. **Data link / Layer 2** — VLANs, trunks, MAC/ARP tables (`show vlan brief`, `show arp`).
3. **Network / Layer 3** — addressing, routing table, gateway correctness (`show ip route`, `ping`).
4. **Services** — DHCP bindings, DNS resolution (`show ip dhcp binding`, `show hosts`).
5. **Management** — SSH/Telnet reachability (`show ip ssh`, `show users`).

Working bottom-up through the OSI layers isolates faults quickly and is the approach used throughout this lab.
