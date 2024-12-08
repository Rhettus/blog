---
layout: post
title: "Home Server Rack Setup: Compact & Low-Powered"
date: 2024-11-30
categories: home-lab
tags: [server, home-lab, rack, low-power, openhpc, nas, raspberry-pi, zabbix, ansible]
wip: true
---


## Overview

In this post, I’m jotting down my ideas for my home server rack setup that’s designed to be decently **performant**, **compact**, and most importantly, **low-powered**. The goal is to create a **small** and **efficient system** for my home server needs, focusing on **power efficiency**. 

## Physical Rack Setup

### **Rack Dimensions & Structure**
- **8U rack 10"**: Custom 3D-printed panels with a **white color scheme** for improved visibility and a clean aesthetic.
- **3D-printed custom panels** for each **U**, made from a smooth matte white finish to enhance legibility and contrast with labels and displays.

### **Components & Layout**
   - **8-port patch panel**: Positioned at the top with **white custom design** and cable management.
   - **PoE-enabled switch**: A cheap, **managed 8-port 2.5GbE switch** with VLAN support. May need room for a second switch
   - **E-paper screens (1.5")**: Embedded into each U, showing device status and icons, benefiting from the white background for high contrast and easy visibility.
   - **Raspberry Pi nodes (3 units)**: Mounted in **custom white 3D-printed enclosures** with cable management. These will serve as nodes for my **OpenHPC cluster**.
   - **NAS (WayPonDEV CM3588 with 4TB NVMe in ZFS)**: Positioned at the bottom with 12Tb usable in RAIDZ1 running OMV
   - **6-outlet PDU**: Mounted at the rear vertically, providing power to the entire rack.

### **Power Considerations**
- **PoE Powering**: The **Raspberry Pi nodes** are powered via **PoE**, reducing the need for additional power adapters and cables.
- **Efficient Power Distribution**: A **6-outlet PDU** efficiently manages power, providing power-saving options.
- **UPS**: Possible to mount a small UPS on the bottom off the unit with DIN rails
- **Low-Power Components**: The **PoE-powered Raspberry Pi units**, **WayPonDEV CM3588 NAS**, and **PoE switch** are selected for their low-power characteristics, minimizing the overall energy consumption.
- **

### **Design & Aesthetic**
- **White 3D-Printed Panels**: Clean, minimalist design with white 3D-printed panels, keeping everything visually organized.
- **Efficient Cooling**: Natural airflow with **ventilation slots** in the 3D-printed panels to keep components cool with options of active fans, which will be turned on when components become hot.

### **Overall Design Summary**
This rack setup is built to be **compact**, **low-powered**, and **performant**. By focusing on **PoE**, **low-power components**, and a **custom design**, the system is both efficient and scalable for future upgrades.

---

## Software Stack

Here’s the **software stack** that will run on this hardware setup:

### **1. OpenMediaVault (OMV) with ZFS**
- **OMV** will serve as the base operating system for the home server, allowing for **flexible storage management**. I will use **ZFS** for the NAS, ensuring reliable data protection and performance with the **WayPonDEV CM3588** and a 4TB NVMe drive.

### **2. Home Assistant**
- **Home Assistant** will be used for **home automation**, integrating various smart devices in my home, and allowing for central control of z-wave devices, powered monitoring, and more.

### **3. WireGuard/Tailscale**
- **WireGuard** or **Tailscale** will provide **secure remote access** to the server and other devices, ensuring encrypted communication while reducing the need for complex VPN setups. This can run on Home Assistant as a container

### **4. UniFi Controller**
- The **UniFi Controller** will manage my network infrastructure, including **Wi-Fi access points** and **network switches**, allowing for **easy network monitoring** and management.  This can run on Home Assistant as a container

### **5. Open HPC Cluster**
- A **Raspberry Pi 4 cluster** running **OpenHPC** will handle **distributed computing** tasks. The cluster will include a **control unit** and **3 worker nodes**, connected via a **PoE-powered switch** for efficient power usage.

### **6. Scrypted**
- **Scrypted** will be used for **video surveillance** and **camera integrations**, particularly useful in monitoring my home, especially with devices like security cameras or smart doorbells.

### **7. Nextcloud**
- **Nextcloud** will act as my **cloud storage solution**, allowing me to sync files, photos, and other data across devices and provide secure file sharing to external users.

### **8. Zabbix**
- **Zabbix** will provide **monitoring** for all devices in the server rack, tracking performance, resource usage, and system health to ensure everything is running optimally. It will be used for **alerting** on issues like disk usage, memory usage, and network activity.

### **9. Ansible**
- **Ansible** will be used for **automated configuration management** and **deployment**. It will simplify setting up new nodes in my **OpenHPC cluster** and help automate other administrative tasks across the server and network infrastructure.

### **10. Pi-hole**
- **Pi-hole** will be installed to provide **network-wide ad-blocking** and improve network security. It will block unwanted content at the DNS level, reducing ads, tracking, and malicious websites across all devices connected to the network.

---

## Physical Layout of 10" Rack
| U     | Equipment                  |
|-------|----------------------------|
| **1U** | Firewall                  |
| **2U** | Switch (Core) (8 Port PoE)|
| **3U** | Patch Panel               |
| **4U** | Switch (2.5Gb PoE)        |
| **5U** | Pi (2U Unit)              |
| **6U** |                           |
| **7U** | NAS                       |
| **8U** | Drawer                    |

---

## Final Thoughts

This setup is designed to meet the needs of my home server environment while being mindful of both **space** and **power**. By using **low-power devices**, **PoE** for easy cabling, and efficient components like the **Raspberry Pi** and **ZFS**, this home lab offers a **performant**, **compact**, and **power-efficient** solution for all my server needs.

The combination of **hardware** and **software** creates a **scalable**, **secure**, and **easily manageable** home server environment that can grow with my future needs.

---

