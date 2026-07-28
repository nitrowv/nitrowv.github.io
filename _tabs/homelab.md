---
layout: page
title: My Homelab
icon: fas fa-server
order: 6
permalink: /homelab/
---
It's more homeproduction than homelab, but here's what I've got running in it.

*As of July 2026*

## Servers

- ### **Titanium (Proxmox)**

	- Intel Xeon E5-2698 v3
	- Asus X99-WS/IPMI
	- 160 GB RAM
	- 128 GB SATA M.2 SSD
	- 1 TB NVMe SSD
  - 800 GB Sun F80 SSD
	- Mellanox ConnectX-2 10Gb NIC
	- Corsair 4000D Airflow

### **OPNsense Firewall**

  - Lenovo M720q
  - Intel Core i5-8400T
  - 8 GB RAM
  - 256 GB SSD
  - Mellanox ConnectX-3 10Gb NIC

### **Iron (TrueNAS)**

  - Intel Core i3-13100T
  - Gigabyte B760I Aorus Pro DDR4
  - 32 GB RAM
  - 128 GB SATA M.2 SSD
  - 2x 200GB Intel S3710 SSDs (Striped)
  - 4x 14TB Seagate/WD HDDs, 28 TB Usable (RAIDZ2)

## Virtual Machines

- #### Krypton (Docker Host)
  - Debian 13
  - 4 vCPUs, 8 GB RAM, 64 GB HDD
  - Docker Services:
    - **General:** Actual Budget, Lubelogger
    - **Homelab Utilities**: Dockhand, Homer, Traefik, Librespeed
    - **Media:** Radarr, Sonarr, Lidarr, Prowlarr, Dispatcharr, Immich
    - **Ham Radio**: Hamclock, Wavelog

## Networking

- OPNsense Firewall
- UniFi Aggregation
- UniFi Pro Max 16 PoE
- UniFi AC Pro Access Point


### OPNsense Plugins

- acme-client
- ddclient
- mdns-repeater
- theme-rebellion
- wireguard
