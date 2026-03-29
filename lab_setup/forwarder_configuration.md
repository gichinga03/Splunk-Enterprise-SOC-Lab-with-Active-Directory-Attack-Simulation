# Forwarder Configuration

## Project Purpose

Splunk Universal Forwarder was deployed on all Windows endpoints to forward security logs into the central Splunk server.

This enables continuous event collection from all monitored hosts.

---

## Deployment Scope

### Installed Hosts

* MAXWELL
* GICHINGA
* MWAURA

---

## Forwarder Role

The forwarder performs:

* Local event collection
* Secure forwarding to Splunk server
* Minimal resource usage

---

## Target Splunk Server

### Destination

```text id="t6v8pc"
MAXWELL:9997
```

Port 9997 was configured as the Splunk receiving port.

---

## Receiving Port Configuration

Configured on Splunk server.

### Splunk Listener Port

```text id="h4n1yb"
9997
```

---

## Forwarded Log Sources

### Windows Security Logs

Captures:

* Logon attempts
* Account changes
* Privilege use

### System Logs

Captures:

* Service activity
* Driver events

### Application Logs

Captures:

* Application errors
* Execution issues

### Sysmon Logs

Captures advanced endpoint telemetry.

---

## inputs.conf Configuration

Example configuration:

```ini id="j7m2fd"
[WinEventLog://Security]
disabled = 0
index = winlogs

[WinEventLog://System]
disabled = 0
index = winlogs

[WinEventLog://Application]
disabled = 0
index = winlogs

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = winlogs
```

---

## outputs.conf Configuration

Forwarding destination:

```ini id="q2z5lm"
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = MAXWELL:9997
```

---

## Service Verification

Forwarder service verified after installation.

### Command

```powershell id="u8k3ra"
Get-Service SplunkForwarder
```

Expected result:

```text id="n5p7wc"
Running
```

---

## Validation in Splunk

Events confirmed using:

```spl id="p9d4xy"
index=winlogs host=GICHINGA
```

---

## Security Benefit

Forwarders ensure all endpoint activity reaches the SIEM without relying on manual log export.

---

## ISO/IEC 27001 Alignment

Supports:

### Centralized Monitoring

Unified event visibility

### Audit Logging

Continuous log preservation

### Security Operations

Detection readiness

---

## Screenshots

Forwarder setup and event validation screenshots stored in:

```text id="m3f6ka"
reports/Forwarder_Configuration.pdf
```

---

## Next Stage

The next phase includes:

* Detection queries
* Attack simulation
* Incident reporting
