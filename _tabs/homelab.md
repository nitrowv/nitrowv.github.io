---
layout: page
title: My Homelab
icon: fas fa-server
order: 6
permalink: /homelab/
---
It's more homeproduction than homelab, but here's what I've got running in it.

*Last Updated March 11, 2022*

## Servers

### **Proxmox Cluster**

- #### **Indium**

	- Lenovo Thinkpad T420
	- Intel Core i7-2620M
	- 8 GB RAM
	- 500 GB HDD

- #### **Titanium**

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

- OPNsense Firewall
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

- ddclient
- mdns-repeater
- theme-rebellion
- wireguard
