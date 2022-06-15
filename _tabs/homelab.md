---
layout: page
title: My Homelab
icon: fas fa-server
order: 6
permalink: /homelab/
---
It's more homeproduction than homelab, but here's what I've got running in it.

*Last Updated June 14, 2022*

## Servers

### **Proxmox Cluster**

- #### **Indium (Networking)**

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

## Virtual Machines

#### **Indium**

- #### Hydrogen
  - Debian 11
  - 2 vCPU, 4 GB RAM, 32 GB HDD
  - Docker Services: Pi-hole, Traefik, UniFi Controller, Portainer Agent

#### **Titanium**

- #### Krypton
  - Ubuntu Server 20.04 LTS
  - 4 vCPUs, 8 GB RAM, 500 GB HDD
  - Docker Services:
    - **General:** Gluetun, Homer, Portainer
    - **Media:** Bazarr, Jackett, Jellyfin, qBittorrent, Radarr, Sonarr

- #### Tantalum
  - Windows Server 2019
  - 2 vCPUs, 2 GB RAM, 100 GB HDD
  - Domain Controller for AD Lab


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

- acme-client
- ddclient
- mdns-repeater
- theme-rebellion
- wireguard
