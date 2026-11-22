# Additional Group Policy Deployments & Security Hardening

> Security & Privacy Notice:  
> Network configurations, IP addresses, domain names, and host identifiers in this documentation have been fully masked (`192.168.xxx.xxx` / `lab.local`) in accordance with security best practices for public portfolio sharing.

---

## Overview

This document details additional Group Policy deployments focused on security hardening within the Active Directory domain environment (`lab.local`). These policies were created on the domain controller (`DC01`) and validated on the domain-joined Windows 11 workstation (`WS01`).

The scenarios reflect common enterprise controls used to reduce attack surface and enforce baseline security settings. The button "create a GPO in the domain, and Link it here..." will be used often here.

<img width="372" height="47" alt="create a gpo" src="https://github.com/user-attachments/assets/3ab6ea74-351a-42d7-8152-40ac9fa8f793" />

---

## Task 1: Disable USB Mass Storage

- **Ticket Request:** Management wants to prevent users from connecting USB storage devices to reduce the risk of data exfiltration.
- **Execution Steps:**
  1. Opened Group Policy Management (`gpmc.msc`) on DC01.
  2. Created and linked a new GPO named `Disable-USB-Storage-GPO`.
  3. Edited the GPO and navigated to:  
     `Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access`
  4. Enabled **All Removable Storage classes: Deny all access**.
  5. Linked the GPO to the domain.
- **Verification & Testing:**
  1. Ran `gpupdate /force` on WS01.
  2. Confirmed via `gpresult /r` that `Disable-USB-Storage-GPO` appeared under Applied Group Policy Objects.
  3. (Note: Initial application failed due to a one-hour time skew between DC01 and WS01. After synchronizing the clocks, the policy applied successfully.)
 
<img width="283" height="64" alt="Applied Group Policy Objects" src="https://github.com/user-attachments/assets/98de01b8-b09d-4108-b2b7-2cc7f2b3e723" />


---

## Task 2: Enforce Screen Lock After Inactivity

- **Ticket Request:** Security requires workstations to automatically lock after a short period of inactivity.
- **Execution Steps:**
  1. Created and linked a new GPO named `Screen-Lock-GPO`.
  2. Configured the following under User Configuration → Policies → Administrative Templates → Control Panel → Personalization:
     - Enable screen saver = Enabled
     - Password protect the screen saver = Enabled
     - Screen saver timeout = 60 seconds (for lab testing)
  3. Additionally configured under Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options:
     - Interactive logon: Machine inactivity limit = 60 seconds
    
 <img width="439" height="21" alt="Screenshot 2026-11-22 145751" src="https://github.com/user-attachments/assets/da745392-69fa-483c-b5fa-e686288feb62" />

- **Verification & Testing:**
  1. Ran `gpupdate /force` on WS01.
  2. Signed out and back in.
  3. Confirmed the workstation automatically locked after the inactivity period and required domain credentials to unlock.
<img width="632" height="140" alt="gpudate" src="https://github.com/user-attachments/assets/456428be-1123-4c77-8f82-951143aaaeb1" />

---

## Task 3: Disable Guest Account & Restrict Blank Passwords

- **Ticket Request:** Ensure the built-in Guest account remains disabled and local accounts cannot use blank passwords except at the console.
- **Execution Steps:**
  1. Created and linked a new GPO named `Restrict-Local-Accounts-GPO`.
  2. Configured the following under Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options:
     - Accounts: Guest account status = Disabled
     - Accounts: Limit local account use of blank passwords to console logon only = Enabled
<img width="422" height="45" alt="Disable Accounts" src="https://github.com/user-attachments/assets/01fae29f-128e-409a-8edd-dff64a69c40e" />

- **Verification & Testing:**
  1. Ran `gpupdate /force` on WS01 and rebooted.
  2. Opened Computer Management → Local Users and Groups → Users and confirmed the Guest account is disabled.

---

## Key Technical Skills Demonstrated

- Authoring and linking security-focused Group Policy Objects
- Removable media restrictions, session locking, and local account hardening
- Troubleshooting Group Policy application issues (including time synchronization)
- Policy verification using `gpupdate`, `gpresult`, and functional testing

---
