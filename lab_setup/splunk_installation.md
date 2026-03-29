# Splunk Installation

## Project Purpose

Splunk Enterprise was deployed as the central Security Information and Event Management platform for log aggregation, search, correlation, and incident visibility across the lab environment.

The objective is to collect endpoint and server telemetry from Windows systems and analyze security events generated during attack simulations.

Splunk provides:

* Centralized log collection
* Search and investigation capability
* Detection rule creation
* Security event correlation
* Incident visibility

---

## Deployment Host
 . initial on ubuntu server 
 <img width="3840" height="2160" alt="Screenshot from 2026-03-23 08-13-06" src="https://github.com/user-attachments/assets/bac70415-15f6-4aeb-8c3d-9df7030b7659" />


### Installed On

* MAXWELL

MAXWELL serves as:

* Domain Controller
* Splunk Server
* Primary log collector

---

## Installed Version

### Splunk Enterprise

Installed using the official Splunk Enterprise package for Windows.

---

## Installation Method

Standard installation completed with administrative privileges.

### Default Management Port

```text id="f3m9qe"
8000
```

### Web Access

```text id="v9p2nk"
https://MAXWELL:8000
```

---

## Initial Configuration

After installation:

* Administrator account created
* License accepted
* Web interface initialized

---

## Splunk Index Design

A dedicated index was created for Windows event ingestion.

### Index Created

```text id="r5j7wa"
winlogs
```

Purpose:

* Separate security logs from system logs
* Simplify searches
* Improve rule organization

---

## Data Sources Expected

Splunk receives logs from:

* Windows Security Logs
* System Logs
* Application Logs
* Sysmon Operational Logs

---
<img width="3063" height="1434" alt="Screenshot from 2026-03-23 08-16-37" src="https://github.com/user-attachments/assets/ee19883f-7ff9-4255-be25-6e4c1d024633" />


## Search Validation

Initial searches were performed to confirm ingestion.

### Example Query
<img width="3824" height="2156" alt="Screenshot from 2026-03-23 09-48-33" src="https://github.com/user-attachments/assets/30d38cc9-9235-4adf-87f6-1256d7a1afa5" />


```spl id="d8u4lp"
index=winlogs
```
<img width="3824" height="2156" alt="Screenshot from 2026-03-24 13-49-10" src="https://github.com/user-attachments/assets/b7650bc0-10b4-4f54-b7d7-e24c1d3b4579" />
<img width="3824" height="2156" alt="Screenshot from 2026-03-24 13-50-56" src="https://github.com/user-attachments/assets/1ebe320a-814d-49f2-9b31-72bde6f68d43" />

This confirms incoming events from endpoints.

---

## Security Use Cases Enabled

Splunk supports detection of:

* Failed logons
* Privilege escalation
* PowerShell abuse
* Remote execution activity
* Network-based attack behavior

---

## Dashboard Purpose

Dashboards can be created to monitor:

* Authentication events
* Endpoint activity
* Sysmon telemetry
* Suspicious process launches

---

## ISO/IEC 27001 Alignment

This deployment supports:

### Logging and Monitoring

Centralized event visibility

### Incident Detection

Real-time event analysis

### Audit Trail Preservation

Retention of security events

---



---

## Next Stage

The next phase connects endpoints using Splunk Universal Forwarder.
