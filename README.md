# Enterprise Active Directory Security Monitoring Lab

## Project Overview

This project simulates an enterprise Windows security environment designed for centralized monitoring, endpoint visibility, and attack detection.

The lab combines Active Directory, endpoint telemetry, and SIEM monitoring to demonstrate how security events can be collected, analyzed, and investigated in a controlled domain environment.

The objective is to build a practical security monitoring environment aligned with enterprise security operations and selected ISO/IEC 27001 security controls.

---

## Core Technologies

* Active Directory Domain Services
* Windows Server
* Sysmon
* Splunk Enterprise
* Splunk Universal Forwarder
* Kali Linux

---

## Lab Architecture

```text id="z1q5vc"
Kali Linux
   ↓
Windows Workstations (GICHINGA / MWAURA)
   ↓
Sysmon + Windows Event Logs
   ↓
Splunk Universal Forwarder
   ↓
Splunk Enterprise on MAXWELL
```

---

## Infrastructure

### Domain Controller

* MAXWELL

### Workstations

* GICHINGA
* MWAURA

### Attacker Machine

* Kali Linux

### Domain

```text id="f3u9xr"
corp.local
```

---

## Project Components

### Active Directory Deployment

Includes:

* Domain creation
* Organizational Unit design
* Group Policy enforcement
* Domain joining

Documentation:

```text id="h7m2kd"
lab_setup/active_directory_setup.md
```

---

### Sysmon Deployment

Includes:

* Endpoint telemetry collection
* Process monitoring
* Network event logging
* Registry visibility

Documentation:

```text id="r4w8pn"
lab_setup/sysmon_installation.md
```

---

### Splunk SIEM Deployment

Includes:

* Centralized log ingestion
* Index creation
* Search validation

Documentation:

```text id="y6n1bt"
lab_setup/splunk_installation.md
```

---

### Universal Forwarder Configuration

Includes:

* Windows event forwarding
* Sysmon log forwarding
* Endpoint log centralization

Documentation:

```text id="k9d5qa"
lab_setup/forwarder_configuration.md
```

---

## Detection Engineering

Custom Splunk detections were created for common attack patterns.

### Failed Login Detection

```text id="c2v7je"
detection_queries/failed_logins.spl
```

Detects:

* brute-force attempts
* password spraying

---

### PowerShell Detection

```text id="m8x3lf"
detection_queries/powershell_detection.spl
```

Detects:

* suspicious PowerShell execution
* encoded command usage

---

### Lateral Movement Detection

```text id="n4p6sw"
detection_queries/lateral_movement.spl
```

Detects:

* remote network logons
* administrative pivoting

---

## Attack Simulation

Controlled attack simulations were performed from Kali Linux to validate detection coverage.

Documentation:

```text id="u5q1gh"
reports/incident_simulation.md
```

Simulated scenarios:

* failed authentication attempts
* PowerShell execution
* remote logon activity

---

## Security Monitoring Value

This lab demonstrates:

* endpoint visibility
* centralized detection
* event investigation workflow
* enterprise monitoring fundamentals

---

## ISO/IEC 27001 Alignment

The environment supports selected security control areas:

### Access Control

* centralized authentication
* domain policy enforcement

### Logging and Monitoring

* event collection
* SIEM ingestion

### Operational Security

* detection validation
* incident visibility

---

## Repository Structure

```text id="p7k4za"
Enterprise-AD-Security-Lab/
├── README.md
├── lab_setup/
│   ├── active_directory_setup.md
│   ├── sysmon_installation.md
│   ├── splunk_installation.md
│   ├── forwarder_configuration.md
│
├── detection_queries/
│   ├── failed_logins.spl
│   ├── powershell_detection.spl
│   ├── lateral_movement.spl
│
├── reports/
│   ├── incident_simulation.md
│
├── screenshots/
```

---

## Screenshots

The screenshots folder contains evidence of:

* Active Directory configuration
* Sysmon events
* Splunk searches
* Detection results

---

## Future Improvements

Planned expansion:

* MITRE ATT&CK mapping
* Splunk dashboards
* executive incident reporting
* additional attack simulations

---

## Security Focus

This project demonstrates practical SOC-oriented monitoring using enterprise Windows infrastructure.

