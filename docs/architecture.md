# Proxmox Virtualized IT Support Specialist Homelab

**Version:** 0.1  
**Author:** Micah Thompson
**Created:** 2025-11-26
**Last updated:** 2025-11-26

---

## 1. Purpose

The purpose of this project is to design, deploy, and operate a **virtualized enterprise-style IT environment** using Proxmox VE.  
This lab simulates real-world responsibilities of an **IT Support Specialist**, including:

- Windows workstation deployment and troubleshooting  
- Active Directory (AD DS), DNS, and Group Policy management  
- Network segmentation using pfSense firewall + VLANs  
- Linux server administration  
- User account provisioning and access control  
- Documentation of IT processes and troubleshooting  
- Exposure to virtualization and multi-OS environments  

This environment mirrors a small-to-medium business network and demonstrates the ability to support, secure, and manage Windows and Linux systems in enterprise-style conditions.

---

## 2. Scope & Assumptions

### **In Scope**
- Proxmox VE hypervisor on dedicated hardware (HP EliteDesk 800 G3 Desktop Mini) 
- pfSense firewall/router VM  
- Windows Server Domain Controller (AD DS, DNS, Group Policy)  
- Windows 10/11 workstation clients joined to the domain  
- Ubuntu Server providing selected services  
- Network segmentation using VLANs  
- AD user/computer structures for HR, Sales, and IT  
- Documentation of troubleshooting scenarios and mock helpdesk tickets  

### **Out of Scope**
- MDM platforms (Intune, JAMF)  
- Proxmox high availability clustering  
- SAN or distributed storage  
- Advanced SIEM ingestion (covered in separate SIEM project, but likely to be incorporated soon for permanent proxmox use)  

### **Assumptions**
- Proxmox host has 16–32GB RAM  
- Internal DNS is managed by the Domain Controller  
- All workloads operate within a closed/offline network  
- Role-based access is controlled through AD groups  
- VLANs are trunked through pfSense → Proxmox bridge  

---

## 3. Architecture Overview

### 3.1 Proxmox VE

- Bare-metal hypervisor  
- Hosts all VMs and manages compute/storage/network resources  
- Web GUI access (VNC/console included)  
- Local backups and snapshots every day @ 00:00. Retention: 30d.  

---

### 3.2 pfSense Firewall

- Layer 3 router for all VLANs  
- DHCP server 
- DNS forwarder to the Domain Controller  
- VLAN interface creation + tagging  
- Firewall rule enforcement between departments  

**VLANs / Subnets (example design):**

- **VLAN 10:** Servers — `10.0.10.0/24`  
- **VLAN 20:** HR Workstations — `10.0.20.0/24`  
- **VLAN 30:** Sales Workstations — `10.0.30.0/24`  
- **VLAN 40:** IT Workstations — `10.0.40.0/24`  
- **VLAN 99:** Proxmox Management — `10.0.99.0/24`  

---

### 3.3 Windows Server 2022 – Domain Controller

- Domain: `lab.local`  
- Services:
  - Active Directory Domain Services  
  - DNS  
  - Group Policy Management 

lab.local
├── HR
│ ├── Users
│ └── Computers
├── Sales
│ ├── Users
│ └── Computers
└── IT
├── Users
└── Computers