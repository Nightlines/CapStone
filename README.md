# Government / Municipal

Services / Infrastructure Project

## 🏛️ Executive Summary
This repository contains the architecture, configuration, and compliance evidence for the Government / Municipal Services Infrastructure Project. Designed and deployed over a 15-week lifecycle, this enterprise-grade network serves 100,000 citizens across Police, Fire/EMS, Public Works, and City Administration departments. 

The infrastructure is built on a **Security-First, Deny-by-Default** methodology, heavily balancing public transparency (FOIA/Citizen Services) with strict FBI Criminal Justice Information Services (CJIS) compliance.

### 👥 Team 6
* **Cameron O’Brien** - Project Manager
* **Fiston Kayembe** - Network Lead
* **Rita Oppong** - Security Lead
* **Kayla Cornett** - Documentation Lead

---

## 📂 Repository Structure
This repository acts as the primary evidence bundle for auditing and disaster recovery. It strictly follows a standardized directory structure:

* 📁 `/configs` - Sanitized running and startup configurations for all routers, switches, and firewalls.
* 📁 `/screenshots` - Visual evidence of successful tests, configurations, and topology states.
* 📁 `/logs` - CLI outputs (`show` commands), ping matrices, traceroutes, and event logs.
* 📁 `/scripts` - Python automation scripts for config backups and cloud integration.
* 📁 `/reports` - Weekly PDF deliverables, Charters, Rebuild Guides, and Compliance Matrices.
* 📁 `/matrix` - Access Control Lists (ACL) mappings and inter-VLAN routing test matrices.

---

## 🏗️ Architecture Overview

### 1. Network Segmentation (Layer 2 & 3)
The network utilizes Variable Length Subnet Masking (VLSM) and 802.1Q trunking to enforce strict isolation. Inter-VLAN routing is handled by the Core Layer 3 Switch (SPF-CSW-01) utilizing Switched Virtual Interfaces (SVIs) and OSPF Area 0.

| VLAN ID | Zone | Subnet | Role | Security Posture |
| :--- | :--- | :--- | :--- | :--- |
| **10** | Public Safety | `10.60.10.0/24` | Police, Fire, CAD Systems | CJIS (High Trust) |
| **20** | Muni Admin | `10.60.20.0/24` | City Hall, HR, Public Works | Internal (Medium Trust) |
| **30** | Management | `10.60.30.0/26` | IT Administration & Syslog | Restricted |
| **40** | DMZ | `10.60.40.0/26` | Citizen Web Portal | Low Trust / Public-Facing |
| **50** | Public WiFi | `10.60.50.0/23` | Guest Kiosks | Untrusted / Isolated |
| **99** | Native/Parking | N/A | Unused Port Isolation | Blackhole |

### 2. Identity & Systems Integration (Windows Server)
Enterprise identity management is driven by Windows Server 2019 Active Directory Domain Services (AD DS).
* **Domain:** `springfield.local`
* **RBAC & OUs:** Strict Organizational Unit separation between `City_Admin` and `Public_Safety`.
* **Group Policy (GPOs):** Enforced 14-character passwords, 90-day rotation, Account Lockout thresholds (5 attempts/30 mins), and comprehensive Logon Auditing.
* **Records Management:** Secured file shares utilizing NTFS permissions and Access-Based Enumeration (ABE) for CJIS data.

### 3. Perimeter Security (Cisco ASA Firewall)
A Cisco ASA 5506-X operates as the network perimeter, establishing four primary security zones.
* **Levels of Trust:** `OUTSIDE (0)`, `DMZ (50)`, `INSIDE (80)`, `CJIS (100)`.
* **NAT Posture:** Dynamic PAT for internal outbound traffic; Static NAT for inbound Citizen DMZ access.
* **Access Control:** 15+ explicit ACLs dictating granular flows, ensuring public traffic can never initiate a connection to the CJIS zone, satisfying FBI CJIS Area 5.5 boundary protection.

---

## ⚙️ Automation & Disaster Recovery (DR)
To ensure high availability and rapid recovery of municipal services, the environment is equipped with automated DR workflows.

* **Python Configuration Backups:** The `SPF_Config_Backup.py` script leverages `netmiko` and `paramiko` to autonomously pull and timestamp running configurations from all network appliances.
* **Cloud Integration:** Backup archives are seamlessly pushed to an off-site cloud storage bucket via API, protecting against localized datacenter destruction.
* **Proven RTO:** The network architecture has been successfully tested in a bare-metal rebuild scenario, achieving a Recovery Time Objective (RTO) of under 2 hours utilizing our documented `Rebuild Guide v2.0`.

---

## 📜 Compliance & Auditing
This infrastructure was designed from the ground up to pass rigorous federal and state audits. 
* **FBI CJIS Policy Compliance:** Validated adherence to auditing (5.4), access control (5.5), and authentication (5.6) standards.
* **Evidence-Driven:** Every rule, route, and policy is backed by packet-tracer simulations, hit-counts, and log evidence located in the `/screenshots` and `/logs` directories.

---
*Created for the Capstone Final Demonstration.*
