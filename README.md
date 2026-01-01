# BLUESPAWN:CASE STUDY-Windows-Incident-Response-Analyzing-and-Remediating-T1055-Process-Injection
A technical deep-dive into the detection, analysis, and removal of a multi-stage malware infection. This project demonstrates real-world application of EDR tools (BLUESPAWN), forensic log analysis (CBS/SFC), and system hardening to mitigate document-harvesting threats and restore OS integrity.




# Incident Response Case Study: T1055 Process Injection & Data Harvesting

## 🛡️ Project Overview
This project documents the end-to-end detection, analysis, and remediation of a live malware infection on a Windows endpoint. The threat featured advanced persistence via modified system DLLs and unauthorized data exfiltration services.

## 🔍 Phase 1: Detection
Using **BLUESPAWN (EDR/Hunt Mode)**, I identified several critical Indicators of Compromise (IoCs):
* **Process Injection (T1055):** Malicious code injected into `Steam.exe` and `Overwolf.exe`.
* **System File Corruption:** `kernel32.dll` flagged for modified Yara rules including `anti_dbg` and `vmdetect`.
* **Persistence (T1543.003):** An unsigned service (`MyService1`) linked to a data-harvesting executable (`AdjustService.exe`).

## 🧪 Phase 2: Analysis & Forensic Evidence
* **Data Exfiltration:** The logs confirmed the malware was targeting the `Documents` and `Users` directories.
* **Evasion:** The modified `kernel32.dll` was designed to detect Virtual Machines and hide from security analysts.
* **Log Validation:** Analyzed `CBS.log` to monitor the Windows Resource Protection (SFC) repair transaction.

## 🛠️ Phase 3: Remediation & Recovery
1. **Service Neutralization:** Terminated and deleted the `MyService1` backdoor via CLI (`sc delete`).
2. **OS Integrity Restoration:** Utilized `DISM` and `SFC` to overwrite the compromised `kernel32.dll` with a verified Microsoft version.
3. **Application Patching:** Performed a full integrity flush on the Steam platform to clear injected memory modules.

## 📈 Phase 4: System Hardening (Mitigation)
Based on the BLUESPAWN Audit report, I implemented the following security controls:
* **LSA Protection (M1025):** Enabled to prevent credential dumping from memory.
* **SMBv1 Disablement (V-73519):** Removed legacy protocol to reduce attack surface.
* **Firewall Configuration:** Restored active monitoring on all network profiles.

---
### 📁 Included Logs
* `result_blueswpn.txt`: Original EDR detection logs.
* `CBS.log`: Record of successful system file repairs.
* `mitigate.txt`: Hardening audit results.
