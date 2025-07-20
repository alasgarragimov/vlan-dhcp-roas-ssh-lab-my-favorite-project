# 🛰 vlan-dhcp-roas-ssh-lab-my-favorite-project Network Project – VLAN 10/20/90 + DHCP + SSH + ROAS

Bu layihə Cisco Packet Tracer mühitində VLAN-lar, DHCP, SSH və Router-on-a-Stick (ROAS) konfiqurasiyası ilə yaradılmış real dünya şəbəkə simulyasiyasıdır. Layihə vasitəsilə müxtəlif VLAN-larda olan cihazların dinamik IP alması, təhlükəsiz idarəetmə (SSH), və yönləndirmə sınaqdan keçirilmişdir.

---

## 📌 Topologiya

- 🖥️ 4 PC (VLAN 10 və VLAN 20)
- 🖧 1 Switch (VLAN 90 idarəetmə üçün)
- 🌐 1 Router (ROAS ilə)
- 📡 1 DHCP Server (Üç ayrı IP pool: VLAN10, VLAN20, VLAN90)

---

## 🧠 VLAN və IP Planı

| VLAN  | Rolu          | IP Subnet            | Gateway          |
|-------|---------------|----------------------|------------------|
| 10    | PC1, PC2      | 192.168.10.0/24      | 192.168.10.1     |
| 20    | PC3, PC4      | 192.168.20.0/24      | 192.168.20.1     |
| 90    | Switch, Server| 192.168.90.0/24      | 192.168.90.1     |

---

## ⚙️ Əsas IOS Əmrlər

### 🔹 Switch:
```bash
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 10
 description VLAN10-PC
 spanning-tree portfast

interface range fa0/3 - 4
 switchport mode access
 switchport access vlan 20
 description VLAN20-PC
 spanning-tree portfast

interface fa0/24
 switchport mode trunk
 description trunk-to-router
 switchport nonegotiate

interface vlan 90
 ip address 192.168.90.10 255.255.255.0
 no shutdown

```



 interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.90.2

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.90.2

interface g0/0.90
 encapsulation dot1Q 90
 ip address 192.168.90.1 255.255.255.0

interface g0/0
 no shutdown
Pool Name: VLAN10
Default Gateway: 192.168.10.1
DNS: 8.8.8.8
Start IP: 192.168.10.100
Subnet: 255.255.255.0

*****************

Pool Name: VLAN20
Default Gateway: 192.168.20.1
DNS: 8.8.8.8
Start IP: 192.168.20.100
Subnet: 255.255.255.0

*****************

Pool Name: VLAN90
Default Gateway: 192.168.90.1
DNS: 8.8.8.8
Start IP: 192.168.90.100
Subnet: 255.255.255.0

******************

hostname R1
ip domain-name nova.local
crypto key generate rsa
username nova privilege 15 secret 1234
ip ssh version 2

line vty 0 4
 login local
 transport input ssh
