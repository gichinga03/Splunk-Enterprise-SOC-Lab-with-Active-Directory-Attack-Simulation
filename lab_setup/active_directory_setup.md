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

### GPO Name

```text
Enterprise Security Baseline
```

### Controls Applied

* Password complexity enabled
* Minimum password length configured
* Account lockout threshold configured
* PowerShell logging enabled
* Security auditing enabled

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

---

## Screenshots

Detailed screenshots are included in the reports folder.

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
