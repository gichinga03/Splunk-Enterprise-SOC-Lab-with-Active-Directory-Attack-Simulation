# Sysmon Installation

## Project Purpose

Sysmon (System Monitor) was deployed across Windows systems to provide enhanced endpoint visibility beyond standard Windows Event Logs.

The objective is to capture detailed security telemetry required for threat detection, attack analysis, and SIEM ingestion.

Sysmon provides:

* Process creation visibility
* Network connection monitoring
* Registry activity tracking
* File creation timestamps
* Parent-child process relationships

---

## Deployment Scope

### Installed Hosts

* MAXWELL
* GICHINGA
* MWAURA

---

## Sysmon Package Used

Installed using Sysinternals Sysmon.
<img width="3840" height="2160" alt="Screenshot from 2026-03-21 11-34-15" src="https://github.com/user-attachments/assets/5409a34a-4082-4d33-a831-17034a53c90b" />


### Binary

```text id="x8u5pr"
Sysmon64.exe
```

### Configuration File

```text id="m7q9la"
sysmonconfig.xml
```

---

## Installation Command

Executed with administrative privileges.

```powershell id="v1n3sd"
Sysmon64.exe -accepteula -i sysmonconfig.xml
```

This command:

* Accepts the Sysmon license
* Installs Sysmon as a Windows service
* Loads the custom monitoring configuration

---
<img width="3840" height="2160" alt="Screenshot from 2026-03-21 14-02-36" src="https://github.com/user-attachments/assets/1b8cc4a8-e0a3-4054-b08c-0abcdf385164" />


## Service Verification

After installation, the service was verified.

### Verification Command

```powershell id="t0a8kg"
Get-Service Sysmon64
```

Expected result:

```text id="w6r2nm"
Running
```

---

## Log Location

Sysmon logs are stored in Windows Event Viewer.

### Path

```text id="k5c4yo"
Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational
```

---
<img width="3840" height="2160" alt="Screenshot from 2026-03-21 11-40-10" src="https://github.com/user-attachments/assets/f767f08d-adbd-4bc2-89d2-307e83f68ce7" />


## Key Events Collected

### Process Creation

Captures:

* Executable launched
* Command line arguments
* Parent process

### Network Connections

Captures:

* Source IP
* Destination IP
* Ports used
* Process responsible

### Registry Changes

Captures:

* Key creation
* Value modification

### File Creation Time Changes

Captures:

* Timestamp manipulation attempts

---

## Important Event IDs

### Event ID 1

Process creation

### Event ID 3

Network connection

### Event ID 7

Image loaded

### Event ID 11

File created

### Event ID 13

Registry value set

---

## Detection Value in Security Monitoring

Sysmon significantly improves visibility for detecting:

* PowerShell abuse
* Lateral movement
* Suspicious executable launches
* Network beaconing
* Privilege escalation behavior

---

## Example Validation

A simple test command was executed to confirm logging.

```powershell id="j2f8vn"
notepad.exe
```
<img width="3840" height="2160" alt="Screenshot from 2026-03-21 14-08-38" src="https://github.com/user-attachments/assets/fc384511-2c3f-40a3-966c-89e62440669f" />


This generated a Sysmon Process Creation event.

---

## Splunk Integration Purpose

Sysmon logs are later forwarded into Splunk to support:

* Endpoint detection
* Attack correlation
* Incident investigation

---

## Security Benefit

Sysmon closes gaps left by default Windows logging by providing richer endpoint telemetry required for modern SOC workflows.

---

## ISO/IEC 27001 Alignment

This deployment supports:

### Logging and Monitoring

Enhanced audit trail generation

### Security Event Visibility

Detection of unauthorized activity

### Operational Security

Continuous event collection

---

## Screenshots

Installation screenshots and event samples are stored in:

```text id="n4z7qy"
reports/Sysmon_Installation.pdf
```

---

## Next Stage

The next phase integrates:

* Splunk Enterprise installation
* Universal Forwarder deployment
* Centralized event ingestion
