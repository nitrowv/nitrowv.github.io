---
layout: page
title: My Homelab
icon: fas fa-server
order: 6
permalink: /homelab/
---
It's more of a Home-production, but here's what I've got running in it.

*Last Updated March 8, 2022*

## Servers

### **Krypton (Docker)**

- Lenovo Thinkpad T420
- Intel Core i7-2620M
- 8 GB RAM
- 500 GB HDD
- Ubuntu 21.10
- I'll eventually convert the disk to a VM and make this another Proxmox node.

    **Containers/Services**
  - Guacamole
  - Heimdall
  - Jellyfin
  - Portainer
  - Qbittorrent
  - Wyze-Bridge

### **Titanium (Proxmox)**

- Intel Core i7-7700K
- Asus Maximus VIII Hero
- 16 GB RAM
- 128 GB SATA M.2 SSD
- 2 TB HDD

### **Cadmium (OPNsense Firewall)**

- Intel Xeon E5504
- Supermicro X8STi
- 4 GB RAM
- 160 GB HDD
- 2x Intel 82574L Gigabit NIC
- Mellanox ConnectX-2 10Gb NIC
- Supermicro SC512-200B
  
## Miscellaneous

- Raspberry Pi 3B - `Hydrogen` (UniFi Controller)

## Networking

- OPNsense Firewall (1Gb to WAN, 1Gb [soon to be 10Gb] to LAN)
- Mikrotik CSS326-24G-2S+RM Switch
- UniFi AC Pro Access Point
  
### VLANs/Networks

- ```VLAN 1 - 192.168.1.0/24``` - OPNsense native/LAN
- ```VLAN 5 - 192.168.5.0/24``` - Management (Unifi Controller and AP)
- ```VLAN 6 - 192.168.6.0/24``` - Trusted/Servers
- ```VLAN 7 - 192.168.7.0/24``` - IoT Devices
- ```VLAN 8 - 192.168.8.0/24``` - Client Devices/Guests
- ```VLAN 9 - 192.168.9.0/24``` - WFH Devices  (***To Be Implemented***)

### OPNsense Plugins

- ddclient (IDK what's going on with this but it will not update my DuckDNS)
- mdns-repeater (To make Chromecast work)
- theme-rebellion (Dark mode 4 lyfe)
