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

<img width="1920" height="1080" alt="Screenshot_2026-03-30_01_52_49" src="https://github.com/user-attachments/assets/e9e8c551-2ebf-4f3d-a85b-5624ce8d22a5" />
<img width="1920" height="1080" alt="Screenshot_2026-03-30_01_52_54" src="https://github.com/user-attachments/assets/edf259b1-f513-4d7e-936c-c27f84f3a53f" />


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

<img width="1920" height="1080" alt="Screenshot_2026-03-30_02_21_46" src="https://github.com/user-attachments/assets/7d2d5c6e-6ace-46b5-8f59-3227586894cc" />


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

<img width="1920" height="1080" alt="Screenshot_2026-03-30_02_33_37" src="https://github.com/user-attachments/assets/e5df4316-c046-4f22-adb5-070a1a8feaab" />
<img width="1920" height="1080" alt="Screenshot_2026-03-30_02_36_10" src="https://github.com/user-attachments/assets/453f0141-960f-459d-813a-4b97a1d5cb49" />
<img width="1920" height="1080" alt="Screenshot_2026-03-30_02_45_51" src="https://github.com/user-attachments/assets/9b97be1a-0f18-4a1c-94e5-0c2f76f3cb2a" />



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

\

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

<img width="1920" height="1080" alt="Screenshot_2026-03-30_02_17_41" src="https://github.com/user-attachments/assets/e6acb789-6a1e-4386-9fe4-e50dc82b2598" />


**Splunk Detection:**
* **Log Source:** `Microsoft-Windows-Sysmon/Operational`
* **Query:** Monitored **Sysmon Event ID 10** (ProcessAccess). I looked for the `lsass.exe` process being accessed by an unusual source, or **Event ID 4624** with a `Logon_Type=3` (Network Logon) followed by immediate sensitive command execution.
</details>

---

## 🚀 Conclusion
This lab illustrates how offensive maneuvers translate into specific log entries. By configuring **Sysmon** and tuning **Splunk** queries, these attacks—which often go unnoticed in default configurations—can be detected in near real-time.
