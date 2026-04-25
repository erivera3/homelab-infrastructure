# Homelab Infrastructure Evolution

## From Constraint-Driven Design to Enterprise-Style Administration

---

## Overview

This project documents the evolution of a self-built IT environment supporting real users and services.

The infrastructure was not designed upfront—it evolved through solving real-world constraints:

- ISP router locked to a MAC address
- High power consumption and thermal inefficiency
- Storage fragmentation and duplication
- Networking and DNS complexity
- Real-world deployment and support challenges

The current environment supports ~13 active systems and includes:

- Proxmox virtualization (multiple hosts)
- TrueNAS SCALE (ZFS storage)
- Windows Server 2022 (Active Directory + DNS)
- Ubuntu Server / Desktop
- OPNsense (internal firewall)
- Pi-hole (DNS filtering)
- OpenWRT (edge router + DHCP)
- Managed switching
- Wireless Access Point (OpenWRT)

---

## Engineering Approach

All development followed a consistent pattern:

**Problem → Investigation → Root Cause → Redesign → Outcome**

---

## Current Transitional Architecture

This diagram represents the current transitional state of the network, including ISP limitations, OpenWRT edge routing, internal firewalling, DNS filtering, and virtualized lab services.

<p align="center">
  <img src="./images/Network-home.png" width="700"/>
</p>

<p align="center">
  <em>Current Network Topology — Transitional Design</em>
</p>

```mermaid
flowchart TD
    Internet["Internet"] --> ISP["ISP Router (MAC Locked)"]
    ISP --> OpenWRT["OpenWRT (Edge Router + DHCP)"]

    OpenWRT --> Shield["Nvidia Shield (Isolated)"]
    OpenWRT --> N110["N110 Proxmox Host"]

    N110 --> OPNsense["OPNsense VM (Internal Firewall)"]
    N110 --> PiHole["Pi-hole VM (DNS Filtering)"]

    OPNsense --> Switch["Cisco SG200-08 Managed Switch"]

    Switch --> Proxmox["Main Proxmox Host (Minisforum)"]
    Switch --> TrueNAS["TrueNAS SCALE (Primary Storage)"]
    Switch --> MediaNAS["Media NAS"]
    Switch --> AP["Wireless Access Point (OpenWRT)"]
    Switch --> Clients["Wired Clients"]

    AP --> WiFiClients["Wireless Clients"]

    Proxmox --> DC01["Windows Server 2022 (Domain Controller + AD DNS)"]
    Proxmox --> UbuntuServer["Ubuntu Server"]
    Proxmox --> UbuntuDesktop["Ubuntu Desktop"]
    Proxmox --> Win10["Windows 10 Client"]

    Clients -->|DHCP| OpenWRT
    Clients -->|DNS Query| PiHole
    PiHole -->|Forward Internal| DC01
    PiHole -->|Forward External| Internet

    WiFiClients -->|DNS Query| PiHole
