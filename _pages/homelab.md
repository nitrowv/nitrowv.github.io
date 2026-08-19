---
layout: single
title: Homelab
permalink: /homelab/
toc: true
---
It's more homeproduction than homelab, but here's what I've got running in it.

*As of July 2026*

## Servers

### **OPNsense Firewall**

  - Lenovo M720q
  - Intel Core i5-8400T
  - 8 GB RAM
  - 256 GB SSD
  - Mellanox ConnectX-3 10Gb NIC

### **Titanium (Proxmox)**
  - Intel Xeon E5-2698 v3
  - Asus X99-WS/IPMI
  - 160 GB RAM
  - 128 GB SATA M.2 SSD
  - 1 TB NVMe SSD
  - 800 GB Sun F80 Flash Accelerator
  - Mellanox ConnectX-2 10Gb NIC
  - Corsair 4000D Airflow

### **Iron (TrueNAS)**

  - Intel Core i3-13100T
  - Gigabyte B760I Aorus Pro DDR4
  - 32 GB RAM
  - 128 GB SATA M.2 SSD
  - 2x 200GB Intel S3710 SSDs (Striped)
  - 4x 14TB Seagate/WD HDDs, 28 TB Usable (RAIDZ2)
  - Mellanox ConnectX-3 10Gb NIC
  - Fractal Design Node 304

## Services

### Containerized Services
  - **General:** Actual Budget, Lubelogger
  - **Homelab/Network Utilities**: Dockhand, Homer, Traefik, Librespeed, Technitium DNS, Unifi OS, Gluetun
  - **Media:** Radarr, Sonarr, Lidarr, Prowlarr, Dispatcharr, Immich, Jellyfin
  - **Radio**: Hamclock, Wavelog, Ultrafeeder, PiAware, FlightRadar24 feeder

### Dedicated VMs/Servers
  - **General**: Qbittorrent, GPU Accelerated Windows 11 VM for Fusion
  - **Homelab/Network Utilities**: FreePBX, Ansible Automation Platform, Wazuh
  - **Radio**: SDR++ Server


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
