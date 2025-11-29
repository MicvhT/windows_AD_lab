# Proxmox Virtualized IT Support Specialist Homelab

**Version:** 0.1  
**Author:** Micah Thompson
**Created:** 2025-11-26
**Last updated:** 2025-11-29

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


```bash
lab.local
 ├── HR
 │    ├── Users
 │    └── Computers
 ├── Sales
 │    ├── Users
 │    └── Computers
 └── IT
      ├── Users
      └── Computers
```

### Security Groups:
- HR_Staff
- Sales_Staff
- IT_Admins

---

### 3.4 Windows 10/11 Workstations
- Domain-joined desktop VMs
- Placed into departmental VLANs
- Used to test:
    - Group Policy Objects
    - Login permissions
    - Network troubleshooting
    - DNS/DHCP issues
    - Software deployment
    - Scripted logon/logoff settings
    - Mock IT support tickets

---

### 3.5 Ubuntu Server
Configured for one or more IT-relevant roles:
  - Web server (Nginx / Apache)
  - Syslog server
  - Samba/SMB file share
  - Monitoring platform (Zabbix / Prometheus)
Used to demonstrate Linux administration and cross-platform management.

---

## 4. Lab Capabilities / Demonstrated Skills

### 4.1 Windows Administration
- Building and configuring a Domain Controller
- OU design and delegation
- User/computer provisioning
- Group Policy creation, linking, and troubleshooting
- DNS management within AD
- Domain join troubleshooting

---

### 4.2 Network Administration
- VLAN creation and segmentation for departments
- Inter-VLAN routing through pfSense
- DHCP scopes and reservations
- DNS hierarchy and forwarding
- Firewall rules between HR/Sales/IT/Server networks
- Common troubleshooting (ping, tracert, nslookup)

---

### 4.3 IT Support Operations
- Simulated helpdesk tickets
- Troubleshooting login issues (bad DNS, wrong OU, password expired, lockouts)
- Fixing network misconfigurations
- Resolving Windows Update and software installation issues
- Documenting root cause and resolution steps
- Applying security baselines via GPO

--- 

### 4.4 Linux Administration
- SSH configuration
- System service installation
- Managing permissions and ownership
- Package updates and system maintenance
- Log analysis and troubleshooting

--- 

## 5. Evidence Directory Structure
```bash
evidence/
│
├── proxmox/
│   ├── host-info.txt
│   ├── vm-inventory.txt
│   └── bridge-config.png
│
├── pfsense/
│   ├── vlan-config.png
│   ├── firewall-rules.png
│   └── dhcp-scopes.png
│
├── windows-server/
│   ├── ad-users.png
│   ├── ad-computers.png
│   ├── gpo-list.png
│   └── dns-config.png
│
├── windows-clients/
│   ├── win-hr01-domain-join.png
│   ├── win-sales01-gpo.png
│   └── troubleshooting/
│       └── example-ticket01.png
│
└── ubuntu/
    ├── service-status.png
    └── config-files/
```

---

## 6. Setup Steps (High-Level)

### Step 1 — Install Proxmox
- Install Proxmox VE to bare metal
- Configure `vmbr0` bridge
- Upload pfSense, Windows Server, Windows 10/11, and Ubuntu ISOs

---

### Step 2 — Deploy pfSense Firewall VM
- Configure WAN + LAN interfaces
- Create VLANs (10, 20, 30, 40, 99)
- Create DHCP scopes or relay to AD
- Configure DNS forwarding to the Domain Controller
- Write firewall rules (HR↔Sales blocked, IT open, Servers reachable)

---

### Step 3 — Deploy Windows Server (Domain Controller)
- Install Windows Server 2022
- Add AD DS and DNS roles
- Promote to Domain Controller for lab.local
- Create OUs: HR, Sales, IT (Users + Computers)
- Create groups and user accounts

---

### Step 4 — Join Windows Clients to the Domain
- Deploy Workstation VMs
- Assign to departmental VLANs
- Set DNS = Domain Controller IP
- Join domain
- Move computer objects into correct OU

---

### Step 5 — Deploy Ubuntu Server
- Install Ubuntu Server into VLAN 10
- Assign static IP
- Deploy selected role (web, syslog, SMB, etc.)
- Verify cross-department access and firewall compliance

---

### Step 6 — Apply and Test Group Policies
- Create user-based GPOs for each department
- Create computer-based security GPOs
- Link policies to OUs
- Use gpupdate and gpresult to verify

---

Step 7 — Document IT Support Scenarios
- Create mock trouble tickets (e.g., DNS broken, GPO not applying, locked account)
- Break configurations intentionally
- Diagnose, fix, and document each issue
- Store screenshots and explanations in evidence/

--- 

## 7. Conclusion

This homelab simulates the daily responsibilities of an IT Support Specialist, demonstrating practical experience with:
  - Windows Server & Active Directory
  - Windows workstation support
  - Group Policy management
  - VLANs, routing, and firewall rules
  - Linux server administration
  - Virtualization via Proxmox
  - Documentation and troubleshooting methodology