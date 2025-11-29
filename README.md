# Windows Active Directory Lab
**Windows AD + Windows 10/11 + pfSense + Ubuntu**

Short Description : A virtualized IT Support Engineer homelab built using **Proxmox VE**, featuring pfSense firewalling, VLAN segmentation, Active Directory (Windows Server 2022), domain-joined Windows 10/11 clients, and an Ubuntu Server providing optional IT services. This lab simulates a small-business IT environment and demonstrates real-world skills in workstation support, AD/GPO management, networking, and Linux administration.

For full technical outline, see **architecture.md** in `docs/`.

---

## Quickstart
- See `/docs/architecture.md` to view the complete setup, VM roles, VLAN design, and AD structure.
- High-Level View:
    - Proxmox VE on HP Elitedesk 800 G3 Mini
    - A pfSense VM (WAN/LAN + VLANs 10/20/30/40/99)
    - Windows Server 2022, promote to Domain Controller (lab.local)
    - Windows 10/11 clients and join them to the domain
    - Ubuntu Server for IT services
    - Applied Group Policies and test domain functionality
    - Documented mock support tickets with screenshots in `evidence/`
- See `diagrams/` for illustrations of topology and related images.

---

## Ultimate Goal
The goal of this lab is to build a realistic, production-style IT environment that demonstrates the core skills of an IT Support Specialist/Engineer. By deploying and managing Active Directory, Windows workstations, pfSense networking, and Linux services inside a virtualized Proxmox environment, this project showcases hands-on experience with troubleshooting, user/device management, networking, and system administration.