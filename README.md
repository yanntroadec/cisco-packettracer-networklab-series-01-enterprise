# Cisco Packet Tracer Network Labs Series - 01 Enterprise

**Enterprise-grade hierarchical network infrastructure with VLAN segmentation, centralized routing, and wireless LAN controller.**

[![Network Status](https://img.shields.io/badge/Status-Production-brightgreen)]()
[![Packet Tracer](https://img.shields.io/badge/Packet_Tracer-8.2%2B-blue)]()

---

## Project Overview

Enterprise network deployment featuring:
- **Hierarchical 3-tier architecture** (Core/Distribution/Access)
- **10 VLANs** for department/WiFi segmentation  
- **Centralized WiFi management** with WLC + 3 APs
- **Layer 3 routing** with inter-VLAN routing
- **Centralized DHCP** server
- **NAT for Internet access**
- **Security hardening** (VLAN 1 disabled, Native VLAN 10, Black Hole VLAN)

---

## Network Architecture

```
                    INTERNET (203.0.113.0/24)
                            |
                    [ISR 4321 Router]
                       10.0.0.1
                            |
                    [Core Switch 3560]
                  192.168.10.1 (Layer 3)
                            |
        ┌───────────────────┼───────────────────┐
        |                   |                   |
    [WLC]              [SW-IT-2960]      [SW-CORP-2960]     [SW-HR-ACC-2960]
  .10.100               .10.11              .10.12              .10.13
        |                   |                   |                   |
   [3x APs]            [IT Dept]          [Corporate]          [HR/Accounting]
```

---

## IP Addressing

| VLAN | Name | Network | Gateway | DHCP |
|------|------|---------|---------|------|
| 10 | Management | 192.168.10.0/24 | .1 | Static only |
| 20 | Servers | 192.168.20.0/24 | .1 | Static only |
| 30 | IT Department | 192.168.30.0/24 | .1 | .10-254 |
| 40 | Corporate | 192.168.40.0/24 | .1 | .10-254 |
| 50 | HR | 192.168.50.0/24 | .1 | .10-254 |
| 60 | Accounting | 192.168.60.0/24 | .1 | .10-254 |
| 70 | WiFi-IT | 192.168.70.0/24 | .1 | .10-254 |
| 80 | WiFi-Corporate | 192.168.80.0/24 | .1 | .10-254 |
| 90 | WiFi-HR | 192.168.90.0/24 | .1 | .10-254 |
| 100 | WiFi-Accounting | 192.168.100.0/24 | .1 | .10-254 |

---

## Wireless SSIDs

| SSID | VLAN | Security | AP Location |
|------|------|----------|-------------|
| WiFi-IT | 70 | WPA2-PSK | IT Building |
| WiFi-Corporate | 80 | WPA2-PSK | Corporate Building |
| WiFi-HR | 90 | WPA2-PSK | HR/Acc Building |
| WiFi-Accounting | 100 | WPA2-PSK | HR/Acc Building |

---

## Equipment List

### Core
- **Router:** Cisco ISR 4321 (HQ-RTR-EDGE-4321) - 10.0.0.1
- **Core Switch:** Cisco 3560 (HQ-CORE-SW-3560) - 192.168.10.1

### Access Layer
- **Switch IT:** Cisco 2960 (HQ-SW-IT-2960) - 192.168.10.11
- **Switch Corporate:** Cisco 2960 (HQ-SW-CORP-2960) - 192.168.10.12
- **Switch HR/Acc:** Cisco 2960 (HQ-SW-HR-ACC-2960) - 192.168.10.13

### Wireless
- **WLC:** HQ-WLC-01 - 192.168.10.100
- **AP IT:** HQ-AP-IT-01 - 192.168.10.201
- **AP Corporate:** HQ-AP-CORP-01 - 192.168.10.202
- **AP HR/Acc:** HQ-AP-HR-ACC-01 - 192.168.10.203

### Server
- **Server:** HQ-SRV-01 - 192.168.20.10 (DNS: Internal DNS server)

---

## Security Features

- VLAN 1 disabled  
- Native VLAN 10 on all trunks  
- Black Hole VLAN 999 for unused ports  
- Spanning-Tree PortFast & BPDU Guard  
- NAT for Internet access  
- Service password encryption  
- WPA2-PSK wireless encryption  

---

## Quick Start

1. Open `cisco-packettracer-networklab-series-01-enterprise.pkt` in Packet Tracer
2. Credentials in [`docs/CREDENTIALS.md`](docs/CREDENTIALS.md)
3. Test connectivity: `ping 192.168.10.1`

---

## Documentation

- **[Configuration](docs/CONFIGURATION-GUIDE.md)** - Device configs
- **[Credentials](docs/CREDENTIALS.md)** - Access passwords
- **[Topology](docs/TOPOLOGY.md)** - Network architecture

---

## Stats

- **Devices:** 35+
- **Network Equipment:** 10
- **VLANs:** 10 active
- **Departments:** 4

---

## Author

**Yann Troadec**

- Portfolio: [yanntroadec.com](https://yanntroadec.com)
- LinkedIn: [linkedin.com/in/yanntroadec](https://www.linkedin.com/in/yann-troadec/)
- Email: yann.troadec.5@gmail.com
- GitHub: [@yanntroadec](https://github.com/yanntroadec/)

---

**Last Updated:** 27 October 2025  
**Status:** Finished Version 