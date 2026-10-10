# 04 — Hardware Inventory & Console Setup

All equipment is physical Cisco hardware mounted in the **CRAW Rack 1** lab rack. Device models were confirmed from chassis labels and IOS boot output captured during the session.

## Routers

| Qty | Model | Platform / IOS | Interfaces | Notes |
|-----|-------|----------------|------------|-------|
| 2 | **Cisco 2811** Integrated Services Router | `C2800NM-IPBASE-M`, IOS **12.4(15)T13**, RELEASE SOFTWARE (fc3) | 2 × FastEthernet (Fa0/0, Fa0/1) | 2811 rev 49.46, 512 MB DRAM, 126 MB CompactFlash |

Boot banner captured in `media/console/01-router-boot-ios-version.png`:

```
Cisco IOS Software, 2800 Software (C2800NM-IPBASE-M), Version 12.4(15)T13, RELEASE SOFTWARE (fc3)
Cisco 2811 (revision 49.46) with 514048K/10240K bytes of memory.
2 FastEthernet interfaces
239K bytes of non-volatile configuration memory.
126000K bytes of ATA CompactFlash (Read/Write)
```

## Switches

| Model family | Type | Role in lab |
|--------------|------|-------------|
| **Cisco Catalyst 2960** | Layer 2 access switch | Access ports, VLANs |
| **Cisco Catalyst 3560** | Multilayer (L2/L3) switch | VLANs, trunking, SVIs |
| **Cisco Catalyst 3750** (PoE-48) | Stackable multilayer switch | Access/trunk, stacking |

Switch models are visible in `media/rack/04-catalyst-switch-stack.jpg` and `media/rack/05-cisco-2811-and-catalyst-3560.jpg` (Catalyst 3560 SERIES, Catalyst 3750 SERIES PoE-48).

## Security appliance

| Model | Type | Role |
|-------|------|------|
| **Cisco ASA 5520** | Adaptive Security Appliance (firewall) | Perimeter security baseline (see `configs/firewall/`) |

Visible in `media/rack/02-rack-rear-routers-switches-asa.jpg` and `media/rack/06-rack-rear-asa5520.jpg`.

## Console / management setup

Devices were configured out-of-band through the **console port** — not over the network — which is standard practice for initial bring-up.

| Item | Value |
|------|-------|
| Cable | DB9-to-RJ45 **rollover** (light-blue console cable) |
| Adapter | USB-to-Serial (DB9) |
| Terminal emulator | **PuTTY** |
| Serial port | **COM12** |
| Baud rate | **9600** |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |

> Summarized as **9600 8N1, no flow control** — the Cisco console default.

The live console session is shown in `media/rack/03-console-session-live.jpg`.

## Rack reference photos

| File | Shows |
|------|-------|
| `media/rack/01-craw-rack1-router-interface-labels.jpg` | Router 1 / Router 2 interface IP labels |
| `media/rack/02-rack-rear-routers-switches-asa.jpg` | Rear of routers, switches, ASA 5520 |
| `media/rack/03-console-session-live.jpg` | Laptop running the live PuTTY session |
| `media/rack/04-catalyst-switch-stack.jpg` | Catalyst switch stack |
| `media/rack/05-cisco-2811-and-catalyst-3560.jpg` | Cisco 2811 + Catalyst 3560 |
| `media/rack/06-rack-rear-asa5520.jpg` | Rear view incl. ASA 5520 |
| `media/rack/07-cisco-routers.jpg` | Cisco routers |
