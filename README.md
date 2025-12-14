# windows-enterprise-infrastructure-lab
# Windows Server Full LAB

 Project Overview

This repository documents a complete Windows Server 2022 enterprise lab implemented step by step in a structured, production-oriented manner. The goal of this project is to simulate a realistic corporate Active Directory environment using multiple Windows Server roles, Group Policy, DHCP, DNS, Hyper-V, WDS, security hardening, backup strategies, and load balancing.

All configurations were implemented manually and via documented scripts, with each task captured in numbered and named Word documents corresponding to every project phase.

This lab is suitable for:

- Windows Server administrators
- Junior / Mid-level system engineers
- Hands-on Active Directory learning

---

#Lab Topology

Servers and Clients:

=>3 × Windows Server 2022

  - PDC (GUI)
  - Core (Server Core)
  - ADC (Additional Domain Controller)
  -1 × Windows 10 Client

=>Network:

 -Subnet: `192.168.1.0/24`

----

 Server Roles Summary

| Server | Roles Installed                             |
| ------ | ------------------------------------------- |
| PDC    | AD DS, DNS, DHCP, GPO, Hyper-V Replica, WDS |
| Core   | DHCP Failover, Hyper-V                      |
| ADC    | AD DS, DNS, Backup Target                   |
| Win10  | Domain Client                               |

---

# Step-by-Step Implementation

 1. Domain Environment Setup

* Created a new Active Directory forest with domain name: ABC.LOCAL
* Primary Domain Controller configuration:

  * Computer Name: PDC
  * IP Address: **192.168.1.2/24**
  * DNS: Self

---

 2. Organizational Units, Users, and Groups

* Created separate OUs for each department:

  * HR
  * Sales
  * Finance
  * Dev
  * IT

* Created:

  * Users inside each department OU
  * Security groups per department
  * Assigned users to their respective department groups

---

 3. Group Policy Configuration

 3.1 Password & Account Policies

Configured domain-level security policies:

* Password must be changed every **60 days**
* Minimum password length: **6 characters**
* **Complex passwords enabled**
* Remember last **3 passwords**
* Account lockout:

  * Lock after **4 failed attempts**
  * Lock duration: **1 hour**
* Enabled **Remote Desktop** for all domain machines

 3.2 User & System Restrictions

Applied GPOs to enhance security:

* Prohibited access to:

  * External storage devices
  * Task Manager
  * Control Panel
* Removed:

  * Manage
  * Properties
    from computer context menu

---

 4. DHCP Configuration

 4.1 DHCP Scope

* Scope range: `192.168.1.40` → `192.168.1.230`
* Subnet mask: `/24`
* Default gateway: `192.168.1.1`
* DNS Server: `192.168.1.2`
* Exclusion range: `192.168.1.80` → `192.168.1.85`

 4.2 DHCP Failover (Server Core)

* Configured DHCP Failover on:

  * Computer Name: **Core**
  * IP Address: **192.168.1.5**
  * DNS: `192.168.1.2`

---

 5. Hyper-V and Replication

* Installed Hyper-V role on **Windows Server Core**
* Created a Core virtual machine
* Configured **Hyper-V Replica** between:

  * Primary server: PDC
  * Replica host: Core

---

 6. Windows Deployment Services (WDS)

* Installed Windows 10 on client machines using **WDS**

 6.1 Domain Integration

* Joined Windows 10 client to **ABC.LOCAL** domain
* Added **Dev group** to the local administrators group

---

 7. Printer Management

* Deployed a shared network printer for all domain users
* Applied restrictions:

  * Black & white printing only
  * Available usage time:

    * From 9:00 AM to 4:00 PM only

---

8. File Server & Access Control

# Shared Folder Permissions

* Created a shared folder for:

  * Dev
  * HR

* Permissions:

  * Create files
  * Edit files
  * **Delete not allowed**

 Network Drive Mapping

* Configured **mapped network drives** for Dev and HR users

---

 9. Additional Domain Controller (ADC)

* Promoted a server to **Additional Domain Controller**
* Configured:

  * Alternate DNS
  * IP Address: **192.168.1.3**

---
 10. Backup Strategy

* Configured **full server backup**:

  * Daily at **11:00 PM**
  * Source: Main Server (PDC)
  * Destination: **ADC server**

---

11. DNS Load Balancing

* Implemented DNS round-robin load balancing for:

  * `www.abc.local`
* IP Addresses:

  * `192.168.1.8`
  * `192.168.1.9`

---

## Documentation Structure

* Each task is documented in:

  * A dedicated Word file
  * Clearly named and numbered
  * Includes screenshots and step-by-step explanations


