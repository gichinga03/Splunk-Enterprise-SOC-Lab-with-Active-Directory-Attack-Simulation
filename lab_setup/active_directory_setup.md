# Active Directory Setup

## Project Overview

This lab simulates an enterprise Windows environment designed for centralized security monitoring and attack detection. The objective is to build a domain-based infrastructure aligned with enterprise security practices and selected controls from ISO/IEC 27001.

The environment supports:

* Centralized identity management
* Security policy enforcement through Group Policy
* Controlled workstation management
* Security event generation for SIEM ingestion
* Attack simulation from an isolated attacker machine

---

## Lab Infrastructure

### Domain Controller

* MAXWELL
* Windows Server
* Active Directory Domain Services installed

### Workstations

* GICHINGA IP Address 192.168.50.20
* MWAURA   IP Address 192.168.50.30
  <img width="3820" height="2153" alt="Screenshot from 2026-03-19 18-31-56" src="https://github.com/user-attachments/assets/0cb1fdda-0446-4b66-a5ad-27530dcc0e70" />


### Users 
| First Name | Last Name | Username | Password |
| --- | --- | --- | --- |
| Daniel | Mwangi | dmwangi | Dm#Cedar2026! |
| Grace | Wanjiku | gwanjiku | Gw!Secure2026# |
| Kevin | Otieno | kotieno | Ko@Vault2026$ |


### Attacker Machine

* Kali Linux

---

## Domain Configuration

### Domain Name
<img width="3820" height="2153" alt="Screenshot from 2026-03-19 18-39-05" src="https://github.com/user-attachments/assets/df5aac05-3d47-405c-a58a-05709a8b9cc4" />


```text
corp.cedarsence.com
```

### Active Directory Services Installed

Installed roles:

* Active Directory Domain Services
* DNS Server

Promotion completed to create:

```text
corp.local
```

MAXWELL functions as:

* Domain Controller
* DNS Server
* Authentication Authority

---

## Organizational Unit Design

To separate systems according to enterprise administrative practice, Organizational Units were created.

### OUs Created

* Servers
* Workstations
* Admins
* Patch_Managed
  <img width="1828" height="968" alt="Screenshot from 2026-03-19 22-47-21" src="https://github.com/user-attachments/assets/3023b763-f76e-4edf-9b32-63d0331bc5d3" />


### Final Structure

```text
corp.local
├── Servers
├── Workstations
│   ├── GICHINGA
│   ├── MWAURA
├── Admins
├── Patch_Managed
```

---

## Domain Join Process

Both workstations were joined to the domain.
<img width="3840" height="2160" alt="Screenshot from 2026-03-21 10-02-38" src="https://github.com/user-attachments/assets/da5a4978-0f7a-4263-a59e-c9d39ff55779" />


### Joined Hosts

* GICHINGA
* MWAURA

### Verification

Command used on clients:

```powershell
gpresult /r
```

Verification confirmed domain policy application.

---

## Group Policy Configuration

A baseline security policy was created and linked to the Workstations OU.
<img width="1831" height="953" alt="Screenshot from 2026-03-19 23-12-19" src="https://github.com/user-attachments/assets/d6315760-01ed-4e0e-a941-863b6ee6abd1" />

### GPO Name
<img width="1831" height="953" alt="Screenshot from 2026-03-19 23-23-30" src="https://github.com/user-attachments/assets/4d3b2851-8cbd-4361-9366-216c070cad77" />


```text
Enterprise Security Baseline
```

### Controls Applied

* Password complexity enabled
* <img width="1921" height="1037" alt="Screenshot from 2026-03-19 23-28-18" src="https://github.com/user-attachments/assets/4cc8eb0a-404c-4437-a3cb-50d82efe484b" />
* Minimum password length configured
* Account lockout threshold configured
<img width="2046" height="1011" alt="Screenshot from 2026-03-19 23-31-46" src="https://github.com/user-attachments/assets/8400cec1-5c59-49fe-8d08-2ff475e7ac6d" />
* PowerShell logging enabled
  <img width="2253" height="1014" alt="Screenshot from 2026-03-19 23-53-37" src="https://github.com/user-attachments/assets/e02f9b99-e469-45b2-ac37-9ddc94410219" />
* Security auditing enabled
<img width="2046" height="1011" alt="Screenshot from 2026-03-19 23-39-49" src="https://github.com/user-attachments/assets/b0e4a660-5020-448a-be99-c732135b3411" />

---

## Password Policy

Configured to enforce enterprise account standards.

### Applied Settings

* Minimum password length: 8+ characters
* Password complexity required
* Account lockout after failed attempts

---

## Audit Policy

Security auditing enabled to generate logs for later SIEM collection.

### Audited Events

* Logon events
* Account management
* Policy changes
* Privilege use

---

## PowerShell Logging

Enabled for command visibility and attack detection.

### Logging Enabled

* Module Logging
* Script Block Logging

---

## DNS Validation

Domain name resolution verified from workstations.

### Validation Command

```powershell
nslookup corp.local
```

---

## Security Baseline Purpose

This baseline ensures:

* Consistent workstation configuration
* Reduced attack surface
* Centralized enforcement of enterprise controls

---

## ISO/IEC 27001 Alignment

The Active Directory deployment supports the following control areas:

### Access Control

* Centralized authentication
* Controlled domain membership

### Operations Security

* Logging enabled
* Policy enforcement active

### Change Management

* GPO-based centralized administration
* <img width="3840" height="2160" alt="Screenshot from 2026-03-21 11-12-54" src="https://github.com/user-attachments/assets/ee37fc2d-89e9-4fba-9b95-883bc583d248" />
<img width="3840" height="2160" alt="Screenshot from 2026-03-21 11-34-15" src="https://github.com/user-attachments/assets/4b0fd36b-a1fc-4547-97b4-5b4f87b88ff7" />



---
<img width="3829" height="2084" alt="Screenshot from 2026-03-20 00-11-25" src="https://github.com/user-attachments/assets/4f625b69-caf8-472b-adcc-98d6d1f12346" />



Reference:

```text
reports/Active_Directory_Setup.pdf
```

---

## Next Stage

The next phase integrates:

* Sysmon installation
* Splunk forwarders
* Centralized event collection
* Attack telemetry generation
