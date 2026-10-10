# Network Topology

## Logical diagram (Mermaid)

```mermaid
graph LR
    subgraph LANA["LAN A — 10.0.0.0/8"]
        PCA["PCs (DHCP)"]
        SWA["Catalyst Switch"]
    end
    subgraph LANB["LAN B — 20.0.0.0/8"]
        PCB["PCs (DHCP)"]
        SWB["Catalyst Switch"]
    end
    subgraph LANC["LAN C — 30.0.0.0/8"]
        PCC["PCs (DHCP)"]
    end
    subgraph LAND["LAN D — 40.0.0.0/8"]
        PCD["PCs (DHCP)"]
    end

    R1["Router 1 · Cisco 2811"]
    R2["Router 2 · Cisco 2811"]

    PCA --- SWA
    PCB --- SWB
    SWA -- "Fa0/0 · 10.0.0.2/8" --- R1
    SWB -- "Fa0/0 · 20.0.0.2/8" --- R2
    R1 -- "Fa0/1 · 30.0.0.1/8" --- PCC
    R2 -- "Fa0/1 · 40.0.0.1/8" --- PCD
    R1 == "static routing" === R2
```

## Router-on-a-stick detail (inter-VLAN routing)

```mermaid
graph TD
    R["Router · Cisco 2811"]
    T["802.1Q Trunk<br/>Fa0/0"]
    R --- T
    T --- SW["Catalyst Switch"]

    R ---|"Fa0/0.10 · VLAN 10 STAFF"| V10["VLAN 10"]
    R ---|"Fa0/0.20 · VLAN 20 SERVERS"| V20["VLAN 20"]
    R ---|"Fa0/0.99 · VLAN 99 MGMT"| V99["VLAN 99"]

    SW --- V10
    SW --- V20
    SW --- V99
```

## ASCII reference

```
   LAN A (10.0.0.0/8)                     LAN B (20.0.0.0/8)
        |                                        |
   [Catalyst SW]                            [Catalyst SW]
        | Fa0/0 10.0.0.2/8                       | Fa0/0 20.0.0.2/8
   +----+-----+        static routing       +----+-----+
   | ROUTER 1 |===========================  | ROUTER 2 |
   +----+-----+                             +----+-----+
        | Fa0/1 30.0.0.1/8                       | Fa0/1 40.0.0.1/8
   LAN C (30.0.0.0/8)                     LAN D (40.0.0.0/8)
```

> Interface addressing is taken from the physical rack labels (`media/rack/01-craw-rack1-router-interface-labels.jpg`). See [`../docs/02-lab-topology.md`](../docs/02-lab-topology.md).
