# Active Directory Attack & Detection Lab (Splunk)

This project demonstrates a full-spectrum attack on an Active Directory environment, mapped directly to **Splunk** for detection and analysis. By correlating offensive actions with Windows Event Logs and Sysmon, I identified the digital fingerprints left by common exploit techniques.

---

## 🛠️ Phase 1: Reconnaissance (Nmap)
Identifying the attack surface of the Domain Controller.

<details>
  <summary><b>Click to expand: Nmap Detailed Scan</b></summary>

* **Objective:** Identify open ports and service versions.
* **Action:** Executed an aggressive scan: `nmap -sC -sV -p- <Target_IP>`.
* **Findings:** Discovered Port 88 (Kerberos), 135/445 (RPC/SMB), and 389 (LDAP) were open.

![Nmap Screenshot](PASTE_LINK_TO_IMAGE_HERE)

**Splunk Detection:**
* **Log Source:** `WinEventLog:Security` / Firewall Logs
* **Query:** Looked for **Event ID 5156** (Connection permitted) or **Event ID 5157** (Connection blocked) showing a high volume of connection requests from a single IP address in a short duration.
</details>

---

## 👥 Phase 2: Enumeration (Users & Groups)
Mapping the internal structure of the Domain.

<details>
  <summary><b>Click to expand: User Discovery</b></summary>

* **Objective:** Identify high-privileged accounts (Domain Admins).
* **Action:** Used standard Windows commands and PowerShell to query the AD database.
* **Command:** `net user /domain` and `net group "Domain Admins" /domain`.

![Enumeration Screenshot](PASTE_LINK_TO_IMAGE_HERE)

**Splunk Detection:**
* **Log Source:** `WinEventLog:Security`
* **Query:** Monitored for **Event ID 4661** (A handle to an object was requested), specifically when the `Object Type` is `SAM_USER` or `SAM_GROUP`.
</details>

---

## 📂 Phase 3: SMB Interaction (File Transfer)
Testing the security of network shares for lateral movement or data exfiltration.

<details>
  <summary><b>Click to expand: SMB Upload/Download</b></summary>

* **Objective:** Transfer tools (e.g., Mimikatz) to the target and exfiltrate sensitive files.
* **Action:** Used `smbclient` to connect to the `C$` share.
* **Command:** `put mimikatz.exe` and `get sensitive_data.txt`.

![SMB Screenshot](PASTE_LINK_TO_IMAGE_HERE)

**Splunk Detection:**
* **Log Source:** `WinEventLog:Security`
* **Query:** Filtered for **Event ID 5145** (A network share object was checked). I looked for access to hidden shares (`C$`, `ADMIN$`) from non-administrative source IPs.
</details>

---

## 🎫 Phase 4: Exploitation (Kerberoasting)
Stealing service account hashes without needing administrative privileges.

<details>
  <summary><b>Click to expand: Kerberoasting Attack</b></summary>

* **Objective:** Request a Service Ticket (TGS) to crack offline.
* **Action:** Used Impacket's `GetUserSPNs.py` to identify accounts with SPNs and request the hash.
* **Tool:** `impacket-GetUserSPNs -dc-ip <IP> <DOMAIN>/<USER> -request`.

![Kerberoasting Screenshot](PASTE_LINK_TO_IMAGE_HERE)

**Splunk Detection:**
* **Log Source:** `WinEventLog:Security`
* **Query:** Searched for **Event ID 4769** (A Kerberos service ticket was requested). Specifically looking for **Ticket Encryption Type: 0x17** (RC4), which is a common indicator of Kerberoasting.
</details>

---

## 🔑 Phase 5: Post-Exploitation (SAM Hashes)
Dumping local credentials for persistence and further lateral movement.

<details>
  <summary><b>Click to expand: CrackMapExec SAM Dump</b></summary>

* **Objective:** Extract NTLM hashes from the SAM database.
* **Action:** Leveraged `crackmapexec` with administrative credentials.
* **Command:** `crackmapexec smb <IP> -u Administrator -p 'Password' --sam`.

![SAM Hash Screenshot](PASTE_LINK_TO_IMAGE_HERE)

**Splunk Detection:**
* **Log Source:** `Microsoft-Windows-Sysmon/Operational`
* **Query:** Monitored **Sysmon Event ID 10** (ProcessAccess). I looked for the `lsass.exe` process being accessed by an unusual source, or **Event ID 4624** with a `Logon_Type=3` (Network Logon) followed by immediate sensitive command execution.
</details>

---

## 🚀 Conclusion
This lab illustrates how offensive maneuvers translate into specific log entries. By configuring **Sysmon** and tuning **Splunk** queries, these attacks—which often go unnoticed in default configurations—can be detected in near real-time.
