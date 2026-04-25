# 🖥️ Homelab Infrastructure Evolution  
## From Legacy Hardware to Constraint-Driven Enterprise Design

## Overview

This project documents the full evolution of my homelab environment, starting from repurposed legacy systems and progressing into a modern, virtualized, enterprise-style infrastructure.

This was not built from a tutorial. It evolved through real-world constraints:

- power consumption  
- heat and airflow inefficiencies  
- noise  
- hardware limitations  
- storage sprawl  
- deployment challenges  
- DNS and networking complexity  

The lab simulates a small enterprise environment using:

- Proxmox virtualization  
- TrueNAS SCALE (ZFS storage)  
- Windows Server Active Directory + DNS  
- Ubuntu Server (Linux administration)  
- OPNsense firewall  
- Pi-hole DNS filtering  
- Managed switching  
- Real-world system deployment  

---

## Engineering Approach

Problem → Investigation → Root Cause → Redesign → Outcome  

Every phase in this lab follows this pattern.

---

## Current Architecture

```mermaid
flowchart TD
    Internet[Internet] --> ISP[ISP Router]
    ISP --> Shield[Nvidia Shield]
    ISP --> Firewall[Firewall Appliance<br>OPNsense / Proxmox / Pi-hole]

    Firewall --> Switch[Cisco SG200-08 Managed Switch]

    Switch --> Proxmox[Proxmox Host<br>Minisforum MS-A2]
    Switch --> TrueNAS[TrueNAS SCALE<br>UGREEN DXP4800 Pro]
    Switch --> MediaNAS[Media NAS<br>UGREEN DXP4800]
    Switch --> Clients[Client Devices]

    Proxmox --> DC01[Windows Server 2022<br>DC01 / AD / DNS]
    Proxmox --> UbuntuServer[Ubuntu Server]
    Proxmox --> UbuntuDesktop[Ubuntu Desktop]
    Proxmox --> Win10[Windows 10 Client]
    Proxmox --> PiHole[Pi-hole]


Phase 1 — Dual Legacy Systems

The lab began with two separate legacy systems:

Proxmox host (i7-4790K, 32GB RAM, GTX 970)
Separate storage/NAS system

Actions taken:

Converted desktops into server roles
Removed GTX 970 GPU to reduce heat and power
Ran systems headless

Problems discovered:

High power consumption
Excessive heat
Loud fan noise
Inefficient 24/7 operation
Storage scattered across multiple machines
Phase 1A — Thermal Optimization
Cleaned systems using air blower
Removed dust from fans, heatsinks, and components
Reapplied CPU thermal paste

Outcome:

Lower temperatures
Improved stability
Phase 1B — Airflow Redesign

Original configuration:

2 intake fans
2 exhaust fans
1 large top fan

Problem:

High temperatures despite many fans

Root cause:

Turbulence
No directional airflow
Heat recirculation

Outcome:

Simplified airflow
Reduced noise
Improved cooling efficiency

Key insight:

More fans ≠ better cooling

Phase 2 — Storage Consolidation (UGREEN NAS)

Problem:

HDDs scattered across systems
No centralized storage

Solution:

Purchased UGREEN DXP4800 Pro

Actions:

Backed up original OS using Rescuezilla
Verified disk image
Installed TrueNAS SCALE

Outcome:

Centralized storage
ZFS datasets and snapshots
Organized data management
Phase 3 — Router Evolution
Started with DD-WRT
Transitioned to OpenWRT

Insight:

OpenWRT provides deeper control, package management, and flexibility.

Phase 4 — Firewall Architecture

Hardware:

Intel N150 appliance
4x NICs

Software:

Tested pfSense
Transitioned to OPNsense

Skills learned:

interface mapping
routing
firewall rules
Phase 5 — Virtualized Network Services

Instead of running firewall and DNS directly on hardware:

Installed Proxmox on firewall appliance
Virtualized OPNsense and Pi-hole

Outcome:

Service isolation
Snapshots and rollback
Flexible architecture
Phase 6 — Modern Hardware Upgrade

Problem:

Legacy systems inefficient

Upgrade:

Minisforum MS-A2
32GB DDR5
NVMe storage

Outcome:

Lower power consumption
Less heat and noise
Stable 24/7 operation
Phase 7 — Active Directory & DNS

Setup:

Windows Server 2022
Domain: lab.local
DNS: 192.168.1.20

Work:

Created DNS records
Configured DHCP to use AD DNS
Troubleshot systemd-resolved

Outcome:

Functional internal DNS
Centralized identity
Phase 8 — Real-World Deployment (School Systems)
Reclaimed ~13 decommissioned computers
Removed from legacy Active Directory
Wiped hard drives
Installed Zorin OS

Used for:

Scratch programming
student computing

Outcome:

Real systems deployed to real users
Phase 9 — PXE Deployment Attempt

Goal:

Deploy OS over network

Actions:

Built Ubuntu PXE server
Configured DHCP and TFTP

Problem:

PXE complexity
Time constraints

Solution:

Created 4 USB installers with Balena Etcher
Deployed systems in parallel

Outcome:

Successful deployment

Key insight:

Execution matters more than perfect automation

Skills Demonstrated

Hardware:

thermal optimization
airflow design
GPU removal

Virtualization:

Proxmox
VM management
snapshots

Storage:

TrueNAS
ZFS
disk imaging

Windows:

Active Directory
DNS

Linux:

Ubuntu Server
SSH

Networking:

OpenWRT
firewall configuration
DNS troubleshooting

Deployment:

PXE concepts
USB imaging
real user systems
Next Steps
VLAN segmentation
VPN deployment
IDS/IPS
monitoring/logging
domain-joined clients
Summary

This homelab represents a full infrastructure evolution:

legacy hardware → optimized systems
scattered storage → centralized NAS
consumer networking → firewall architecture
manual installs → deployment strategy

It demonstrates real IT thinking:

identify problems → redesign systems → deliver working solutions

