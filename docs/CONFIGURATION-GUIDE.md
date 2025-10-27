# Configuration Guide

Quick configuration reference for all network devices.

---

## Router Configuration (HQ-RTR-EDGE-4321)

### Basic Setup
```cisco
hostname HQ-RTR-EDGE-4321
enable secret packetlab1
service password-encryption
no ip domain-lookup

banner motd ^C
*********************************************
* HQ-RTR-EDGE-4321 - Internet Edge Router  *
* Unauthorized access is prohibited        *
*********************************************
^C
```

### Interfaces
```cisco
interface GigabitEthernet0/0/0
 description To HQ-CORE-SW-3560
 ip address 10.0.0.1 255.255.255.252
 ip nat inside
 no shutdown

interface GigabitEthernet0/0/1
 description To Internet
 ip address 203.0.113.10 255.255.255.0
 ip nat outside
 no shutdown
```

### NAT & Routing
```cisco
! NAT Configuration
access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 interface GigabitEthernet0/0/1 overload

! Static Routes
ip route 192.168.10.0 255.255.255.0 10.0.0.2
ip route 192.168.20.0 255.255.255.0 10.0.0.2
ip route 192.168.30.0 255.255.255.0 10.0.0.2
ip route 192.168.40.0 255.255.255.0 10.0.0.2
ip route 192.168.50.0 255.255.255.0 10.0.0.2
ip route 192.168.60.0 255.255.255.0 10.0.0.2
ip route 192.168.70.0 255.255.255.0 10.0.0.2
ip route 192.168.80.0 255.255.255.0 10.0.0.2
ip route 192.168.90.0 255.255.255.0 10.0.0.2
ip route 192.168.100.0 255.255.255.0 10.0.0.2
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

---

## Core Switch Configuration (HQ-CORE-SW-3560)

### Basic Setup
```cisco
hostname HQ-CORE-SW-3560
enable secret packetlab1
service password-encryption
no ip domain-lookup

ip routing

banner motd ^C
*********************************************
* HQ-CORE-SW-3560 - Core Switch            *
* Unauthorized access is prohibited        *
*********************************************
^C
```

### VLANs
```cisco
vlan 10
 name Management
vlan 20
 name Servers
vlan 30
 name IT
vlan 40
 name Corporate
vlan 50
 name HR
vlan 60
 name Accounting
vlan 70
 name WiFi-IT
vlan 80
 name WiFi-Corporate
vlan 90
 name WiFi-HR
vlan 100
 name WiFi-Accounting
vlan 999
 name BLACKHOLE

interface Vlan1
 shutdown
```

### DHCP Configuration
```cisco
! Exclusions
ip dhcp excluded-address 192.168.30.1 192.168.30.9
ip dhcp excluded-address 192.168.40.1 192.168.40.9
ip dhcp excluded-address 192.168.50.1 192.168.50.9
ip dhcp excluded-address 192.168.60.1 192.168.60.9
ip dhcp excluded-address 192.168.70.1 192.168.70.9
ip dhcp excluded-address 192.168.80.1 192.168.80.9
ip dhcp excluded-address 192.168.90.1 192.168.90.9
ip dhcp excluded-address 192.168.100.1 192.168.100.9

! Pools
ip dhcp pool IT
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.20.10

ip dhcp pool CORPORATE
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 dns-server 192.168.20.10

ip dhcp pool HR
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.1
 dns-server 192.168.20.10

ip dhcp pool ACCOUNTING
 network 192.168.60.0 255.255.255.0
 default-router 192.168.60.1
 dns-server 192.168.20.10

ip dhcp pool WIFI-IT
 network 192.168.70.0 255.255.255.0
 default-router 192.168.70.1
 dns-server 192.168.20.10

ip dhcp pool WIFI-CORPORATE
 network 192.168.80.0 255.255.255.0
 default-router 192.168.80.1
 dns-server 192.168.20.10

ip dhcp pool WIFI-HR
 network 192.168.90.0 255.255.255.0
 default-router 192.168.90.1
 dns-server 192.168.20.10

ip dhcp pool WIFI-ACCOUNTING
 network 192.168.100.0 255.255.255.0
 default-router 192.168.100.1
 dns-server 192.168.20.10
```

### SVIs (VLAN Interfaces)
```cisco
interface Vlan10
 description Management Gateway
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface Vlan20
 description Servers Gateway
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface Vlan30
 description IT Department Gateway
 ip address 192.168.30.1 255.255.255.0
 no shutdown

interface Vlan40
 description Corporate Gateway
 ip address 192.168.40.1 255.255.255.0
 no shutdown

interface Vlan50
 description HR Gateway
 ip address 192.168.50.1 255.255.255.0
 no shutdown

interface Vlan60
 description Accounting Gateway
 ip address 192.168.60.1 255.255.255.0
 no shutdown

interface Vlan70
 description WiFi-IT Gateway
 ip address 192.168.70.1 255.255.255.0
 ip helper-address 192.168.10.1
 no shutdown

interface Vlan80
 description WiFi-Corporate Gateway
 ip address 192.168.80.1 255.255.255.0
 ip helper-address 192.168.10.1
 no shutdown

interface Vlan90
 description WiFi-HR Gateway
 ip address 192.168.90.1 255.255.255.0
 ip helper-address 192.168.10.1
 no shutdown

interface Vlan100
 description WiFi-Accounting Gateway
 ip address 192.168.100.1 255.255.255.0
 ip helper-address 192.168.10.1
 no shutdown
```

### Trunk Ports
```cisco
! To Router (Layer 3)
interface GigabitEthernet0/1
 no switchport
 ip address 10.0.0.2 255.255.255.252
 no shutdown

! To WLC
interface FastEthernet0/1
 description Trunk to WLC
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,70,80,90,100
 spanning-tree portfast trunk
 no shutdown

! To Server
interface FastEthernet0/2
 description Server VLAN
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown

! To Switch IT
interface FastEthernet0/3
 description Trunk to HQ-SW-IT-2960
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,30,70
 no shutdown

! To Switch Corporate
interface FastEthernet0/4
 description Trunk to HQ-SW-CORP-2960
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,40,80
 no shutdown

! To Switch HR/Acc
interface FastEthernet0/5
 description Trunk to HQ-SW-HR-ACC-2960
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,50,60,90,100
 no shutdown
```

### Unused Ports
```cisco
interface range FastEthernet0/6-24, GigabitEthernet0/2
 switchport mode access
 switchport access vlan 999
 shutdown
```

### Routing
```cisco
ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

---

## Access Switch Configuration (Template)

### Switch Corporate Example
```cisco
hostname HQ-SW-CORP-2960
enable secret packetlab1
service password-encryption
no ip domain-lookup

vlan 10
 name Management
vlan 40
 name Corporate
vlan 80
 name WiFi-Corporate
vlan 999
 name BLACKHOLE

interface Vlan1
 shutdown

interface Vlan10
 ip address 192.168.10.12 255.255.255.0
 no shutdown

ip default-gateway 192.168.10.1

! Trunk to Core
interface FastEthernet0/1
 description Trunk to Core
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,40,80
 no shutdown

! Trunk to AP
interface FastEthernet0/2
 description Link to AP-CORP
 switchport mode trunk
 switchport trunk native vlan 10
 switchport trunk allowed vlan 10,70,80,90,100
 spanning-tree portfast trunk
 spanning-tree bpduguard enable
 no shutdown

! Access Ports
interface range FastEthernet0/3-8
 switchport mode access
 switchport access vlan 40
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown

! Unused Ports
interface range FastEthernet0/9-24
 switchport mode access
 switchport access vlan 999
 shutdown
```

---

## WLC Configuration

### Management Interface
```
IP Address: 192.168.10.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
```

### DHCP Service
```
Service: OFF (Disabled)
```

### WLANs
```
WLAN 1: WiFi-IT
  VLAN: 70
  Security: WPA2-PSK
  Passphrase: WiFi-IT-Pass
  Encryption: AES

WLAN 2: WiFi-Corporate
  VLAN: 80
  Security: WPA2-PSK
  Passphrase: WiFi-Corporate-Pass
  Encryption: AES

WLAN 3: WiFi-HR
  VLAN: 90
  Security: WPA2-PSK
  Passphrase: WiFi-HR-Pass
  Encryption: AES

WLAN 4: WiFi-Accounting
  VLAN: 100
  Security: WPA2-PSK
  Passphrase: WiFi-Accounting-Pass
  Encryption: AES
```

---

## Verification Commands

### General
```cisco
show running-config
show ip interface brief
show vlan brief
show interfaces trunk
show cdp neighbors
```

### DHCP
```cisco
show ip dhcp pool
show ip dhcp binding
show ip dhcp server statistics
```

### Routing
```cisco
show ip route
show ip arp
```

---

**Last Updated:** 27 October 2025