---
layout: page
title: My Homelab
icon: fas fa-server
order: 6
permalink: /homelab/
---
It's more homeproduction than homelab, but here's what I've got running in it.

*As of March 2023*

## Servers

### **Proxmox Cluster**

- #### **Indium (Networking)**

	- Intel NUC5i3SYH
	- Intel Core i3-6100U
	- 4 GB RAM
	- 500 GB HDD

- #### **Titanium**

	- Intel Xeon E5-2698 v3
	- Asus X99-WS/IPMI
	- 128 GB RAM
	- 128 GB SATA M.2 SSD
	- 2 TB HDD
	- Mellanox ConnectX-2 10Gb NIC
	- Corsair 4000D Airflow

### **Cadmium (OPNsense Firewall)**

- Intel Xeon E5504
- Supermicro X8STi
- 4 GB RAM
- 256 GB SSD
- 2x Intel 82574L Gigabit NIC
- Supermicro SC512-200B

### **Iron (TrueNAS)**

- Dell Optiplex 7040 SFF
- Intel Core i5-6500
- 8 GB RAM
- 128 GB SATA M.2 SSD
- 14 TB Seagate Exos X16 HDD

## Virtual Machines

#### **Indium**

- #### Hydrogen
  - Debian
  - 2 vCPUs, 4 GB RAM, 32 GB HDD
  - Docker Services: Pi-hole, Traefik, UniFi Controller, Portainer Agent

#### **Titanium**

- #### Krypton
  - Ubuntu Server
  - 4 vCPUs, 8 GB RAM, 500 GB HDD
  - Docker Services:
    - **General:** Homer, Portainer
    - **Media:** Bazarr, Jellyfin, Radarr, Sonarr

- #### Xenon
  - Ubuntu Server
  - 4 vCPUs, 4 GB RAM, 32 GB HDD
  - Services:
    - **General:** VPN client
    - **Media:** qBittorrent, Jackett

- #### Tantalum
  - Windows Server
  - 2 vCPUs, 2 GB RAM, 100 GB HDD
  - Domain Controller for AD Lab
  
- #### Lithium
  - Rocky Linux
  - 4 vCPUs, 4 GB RAM, 64 GB HDD
  - Ansible/AWX host


## Networking

- OPNsense Firewall
- Mikrotik CSS326-24G-2S+RM Switch
- UniFi AC Pro Access Point


### OPNsense Plugins

- acme-client
- ddclient
- mdns-repeater
- theme-rebellion
- wireguard
