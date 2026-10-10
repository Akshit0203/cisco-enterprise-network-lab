# 01 — Project Overview

## Objective

Design, configure, and verify a small enterprise network on **physical Cisco hardware**, demonstrating practical command-line configuration of routers, switches, and network services, and the ability to troubleshoot connectivity using standard diagnostic tools.

Unlike a simulator-only exercise, every device in this lab was a real appliance consoled into over a serial connection, configured from the Cisco IOS CLI, and verified live.

## Scope

The project is organized around five functional areas:

### 1. Device bring-up and interface addressing
- Access each device via the console port (serial + PuTTY).
- Move through the IOS command modes: user EXEC → privileged EXEC → global configuration → interface configuration.
- Assign IPv4 addresses to router FastEthernet interfaces and bring them up with `no shutdown`.
- Persist configuration with `copy running-config startup-config`.

### 2. Layer 2 switching
- Create VLANs to segment the broadcast domain.
- Assign access ports to VLANs and configure 802.1Q trunk links between switches and the router.
- Verify with `show vlan brief` and `show interfaces trunk`.

### 3. Layer 3 routing
- Implement **inter-VLAN routing** using the router-on-a-stick model (sub-interfaces with 802.1Q encapsulation).
- Configure **static routing** so the separate `/8` networks can reach one another.
- Confirm the routing table with `show ip route`.

### 4. Network services
- Deploy a **DHCP** server on the router to automate client IP assignment, with excluded addresses for infrastructure.
- Configure **DNS** name resolution.

### 5. Secure access and troubleshooting
- Enable **SSH** for encrypted remote management and harden the VTY lines.
- Use **Telnet** to validate reachability.
- Inspect **ARP** tables and use **ICMP (ping)** to analyze and troubleshoot device-to-device communication.

## Skills demonstrated

- Cisco IOS CLI proficiency across EXEC, global, interface, and line configuration modes
- IPv4 addressing and subnetting (classful `/8` segments in this lab)
- Layer 2 concepts: VLANs, access/trunk ports, 802.1Q trunking
- Layer 3 concepts: inter-VLAN routing, static routing, default gateways
- Core services: DHCP, DNS
- Network security: SSH, VTY hardening, AAA-style local authentication
- Structured troubleshooting with `show` commands, ARP, and ICMP
- Reading and acting on IOS log/error messages (e.g., IP overlap, `administratively down`)

## Outcome

A fully reachable multi-segment network in which clients obtain addressing automatically, resolve names, route between segments, and are managed securely over SSH — with each configuration step verified against live device output.

## Context

Conducted during **CCNA training at Craw Security** on the *CRAW Rack 1* lab rack. This repository is a cleaned-up, portfolio-ready reconstruction of that session, built from the live PuTTY console captures and the physical interface labels documented on the rack.
