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

## Search Validation

Initial searches were performed to confirm ingestion.

### Example Query

```spl id="d8u4lp"
index=winlogs
```

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

## Screenshots

Installation screenshots and search validation are stored in:

```text id="e1x6tw"
reports/Splunk_Deployment.pdf
```

---

## Next Stage

The next phase connects endpoints using Splunk Universal Forwarder.
