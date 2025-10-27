# Network Topology

Physical and logical network architecture.

---

## Physical Topology

```
                           INTERNET
                               |
                      [ISR 4321 Router]
                      HQ-RTR-EDGE-4321
                      203.0.113.10 (WAN)
                      10.0.0.1 (LAN)
                               |
                               | Gi0/1 (Routed)
                               |
                      [Core Switch 3560]
                       HQ-CORE-SW-3560
                       192.168.10.1
                       Gi0/1, Fa0/1-5
                               |
              ┌────────────────┼────────────────┐
              |                |                |
         Fa0/1 (Trunk)    Fa0/2 (Access)   Fa0/3-5 (Trunks)
              |                |                |
         [HQ-WLC-01]     [Server VLAN]    [Access Switches]
         .10.100          .20.10
              |
         ┌────┴────┬────────────┐
         |         |            |
    [AP-IT-01] [AP-CORP-01] [AP-HR-ACC-01]
    .10.201     .10.202       .10.203


Access Layer Switches:
─────────────────────

┌─────────────────────┐   ┌──────────────────────┐   ┌───────────────────────┐
│  HQ-SW-IT-2960      │   │  HQ-SW-CORP-2960     │   │  HQ-SW-HR-ACC-2960    │
│  192.168.10.11      │   │  192.168.10.12       │   │  192.168.10.13        │
│                     │   │                      │   │                       │
│  Fa0/1: Trunk       │   │  Fa0/1: Trunk        │   │  Fa0/1: Trunk         │
│  Fa0/2: AP-IT       │   │  Fa0/2: AP-CORP      │   │  Fa0/2: AP-HR-ACC     │
│  Fa0/3-X: IT Users  │   │  Fa0/3-X: Corp Users │   │ Fa0/3-X: HR/Acc Users │
└─────────────────────┘   └──────────────────────┘   └───────────────────────┘
```

---

## Trunk Configuration

### Core Switch Trunks

**Fa0/1 → WLC**
```
Native VLAN: 10
Allowed VLANs: 10,70,80,90,100
Purpose: WLC management + WiFi VLANs
```

**Fa0/3 → Switch IT**
```
Native VLAN: 10
Allowed VLANs: 10,30,70
Purpose: IT Department + WiFi-IT
```

**Fa0/4 → Switch Corporate**
```
Native VLAN: 10
Allowed VLANs: 10,40,80
Purpose: Corporate Dept + WiFi-Corporate
```

**Fa0/5 → Switch HR/Acc**
```
Native VLAN: 10
Allowed VLANs: 10,50,60,90,100
Purpose: HR, Accounting + WiFi-HR, WiFi-Accounting
```

---

## Wireless Topology

### WLC Architecture
```
           [HQ-WLC-01]
          192.168.10.100
                 |
       ┌─────────┼─────────┐
       |         |         |
  [AP-IT-01] [AP-CORP-01] [AP-HR-ACC-01]
   VLAN 70    VLAN 80    VLAN 90/100
```

### AP to SSID Mapping

**HQ-AP-IT-01** 
- Broadcasts: WiFi-IT (VLAN 70)

**HQ-AP-CORP-01** 
- Broadcasts: WiFi-Corporate (VLAN 80)

**HQ-AP-HR-ACC-01** 
- Broadcasts: WiFi-HR (VLAN 90)
- Broadcasts: WiFi-Accounting (VLAN 100)

---

## Layer 3 Routing

### Core Switch (Inter-VLAN Routing)

**SVI (Switched Virtual Interfaces):**
```
interface Vlan10  → 192.168.10.1 (Management)
interface Vlan20  → 192.168.20.1 (Servers)
interface Vlan30  → 192.168.30.1 (IT)
interface Vlan40  → 192.168.40.1 (Corporate)
interface Vlan50  → 192.168.50.1 (HR)
interface Vlan60  → 192.168.60.1 (Accounting)
interface Vlan70  → 192.168.70.1 (WiFi-IT)
interface Vlan80  → 192.168.80.1 (WiFi-Corporate)
interface Vlan90  → 192.168.90.1 (WiFi-HR)
interface Vlan100 → 192.168.100.1 (WiFi-Accounting)
```

**Default Route:**
```
ip route 0.0.0.0 0.0.0.0 10.0.0.1

```

---

## Cable Types

| Connection | Cable Type |
|------------|-----------|
| Router ↔ Core Switch | Straight-through (Gi to Gi) |
| Core ↔ Access Switches | Straight-through (Fa to Fa) |
| Core ↔ WLC | Straight-through |
| Switches ↔ APs | Straight-through |
| Switches ↔ PCs | Straight-through |
| Core ↔ Server | Straight-through |

---

**Last Updated:** 27 October 2025  