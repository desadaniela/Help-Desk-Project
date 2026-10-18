# Active Directory Home Lab — Infrastructure Setup

> **Security & Privacy Notice:** Network configurations, IP addresses, subnet masks, domain names, and host identifiers in this documentation have been fully masked (`192.168.xxx.xxx` / `lab.local`) in accordance with security best practices for public portfolio sharing.

---

## Overview
This repository documents the provisioning, deployment, and configuration of a hands-on **Active Directory Domain Services (AD DS)** home lab environment. This foundation is designed to support administrative management, group policy deployment, and Tier-1/Tier-2 IT support simulations.

---

## Architecture & Topology

| Host Name | Operating System | Role | IP Address | Subnet Mask | Preferred DNS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **DC01** | Windows Server 2022/2025 Standard (Desktop Experience) | Primary Domain Controller / DNS | `192.168.xxx.xxx` | `xxx.xxx.xxx.xxx` | `127.0.0.1` *(Loopback)* |
| **WS01** | Windows 11 Enterprise | Domain Workstation | `192.168.xxx.xxx` | `xxx.xxx.xxx.xxx` | `192.168.xxx.xxx` |

* **Domain Name:** `lab.local` *(Generalized)*
* **Network Isolation:** Host-Only Virtual Network Adapter

---

## Step-by-Step Deployment Guide

### Step 1: Create and Configure the Domain Controller (`DC01`)
1. **Virtual Machine Provisioning:**
   * **OS:** Windows Server (Desktop Experience)
   * **RAM:** Minimum 4 GB (6–8 GB recommended)
   * **Disk:** 60–80 GB dynamic
   * **Network:** Host-Only Adapter (Isolated lab network)
2. **Initial Setup:**
   * Set a strong Administrator password.
   * Rename the computer to `DC01` and reboot.
   * Configure static IPv4 parameters:
     * **IP Address:** `192.168.xxx.xxx`
     * **Subnet Mask:** `xxx.xxx.xxx.xxx`
     * **Preferred DNS:** Pointed to loopback (`127.0.0.1`) after AD installation.
3. **Active Directory & DNS Installation:**
   * Open **Server Manager** → **Add Roles and Features**.
   * Select **Active Directory Domain Services (AD DS)** and accept required management tools.
   * Automatically include the **DNS Server** role.
4. **Domain Controller Promotion:**
   * Promote the server to a domain controller by selecting **Add a new forest**.
   * Set the root domain name to `lab.local`.
   * Configure and securely store the Directory Services Restore Mode (DSRM) password.
   * Complete the wizard and reboot.

---

### Step 2: Establish Active Directory Structure (`dsa.msc`)
Using **Active Directory Users and Computers**, set up a clean organizational layout:
1. **Organizational Units (OUs):**
   * `lab.local` → `Company` → `Users`
   * `lab.local` → `Company` → `Computers`
   * `lab.local` → `Company` → `Groups`
   * `lab.local` → `Company` → `Service Accounts`
2. **User Provisioning:**
   * Created standard test user accounts adhering to a consistent naming convention (e.g., `jdoe`, `asmith`).
3. **Security Groups:**
   * Established core role-based security groups: **Help Desk** and **Standard Users**.

---

### Step 3: Provision and Join Workstation (`WS01`)
1. **Virtual Machine Provisioning:**
   * **OS:** Windows 11 Enterprise
   * **Network:** Host-Only Adapter (matching `DC01`)
2. **Network Configuration:**
   * Set static IP: `192.168.xxx.xxx`
   * Set Preferred DNS to point directly to the Domain Controller (`192.168.xxx.xxx`).
3. **Domain Join:**
   * Navigate to **Settings** → **System** → **About** → **Domain or workgroup**.
   * Join `lab.local` using Domain Administrator credentials.
   * Reboot and verify successful login as a domain user (`LAB\jdoe`).

---

## Key Technical Skills Demonstrated
* **Virtualization & Networking:** Configuring isolated host-only virtual network adapters and static IPv4 addressing.
* **Windows Server Administration:** Deploying server roles, managing local configurations, and promoting a Domain Controller.
* **Directory Services Architecture:** Structuring OUs, managing user object properties, and organizing role-based security groups.
* **Client Systems Integration:** Successfully joining a Windows 11 workstation to an active directory domain and validating DNS resolution.
