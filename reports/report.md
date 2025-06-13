# APT Simulation Report – Phase 3


## Overview

This report summarizes the activities performed during **Phase 3: Advanced Threat Hunting & APT Simulation** of the Cybersecurity Internship Program. The goal was to simulate complex APT scenarios, detect and analyze adversary techniques, and enhance incident response skills using a layered defense approach.

---

## Scenario Summaries

### 1. Fileless Malware via PowerShell

- **Attack Simulation:**  
  Executed a base64-encoded PowerShell command to download and run a remote script.
- **Detection:**  
  - Sysmon Event ID 1 (Process Create) for suspicious PowerShell launches.
  - PowerShell transcript logs reviewed for encoded/obfuscated commands.

---

### 2. Lateral Movement via RDP Brute Force

- **Attack Simulation:**  
  Used Hydra to brute-force RDP credentials from Kali Linux to Windows targets.
- **Detection:**  
  - Security log Event ID 4625 (failed logon) with logon type 10.
  - Account lockout policy tested to mitigate brute force attempts.

---

### 3. Persistence via Registry Run Keys

- **Attack Simulation:**  
  Added an entry under `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run` for persistence.
- **Detection:**  
  - Sysmon Event ID 13 (Registry Value Set) matching “Run” key modifications.
- **Evidence:**  

---

### 4. Data Exfiltration via DNS Tunneling

- **Attack Simulation:**  
  Exfiltrated base64-encoded data within DNS queries to a simulated C2 domain.
- **Detection:**  
  - Zeek logs monitored for DNS queries >100 characters.
  - Custom Sigma rule for base64/long DNS traffic applied.

---

### 5. Credential Dumping and Exfiltration

- **Attack Simulation:**  
  Simulated LSASS process access and fake credential exfiltration over HTTPS.
- **Detection:**  
  - Sysmon Event ID 10 (Process Access) with “lsass.exe” as the target.
  - SIEM monitored for suspicious HTTP POST requests.

---

## Timeline Analysis

- Events were correlated using SIEM dashboards and PowerShell scripts to reconstruct the sequence of attacker actions.
- Key incident timestamps and related artifacts were documented per scenario.

---

## Lessons Learned

- **Most Effective Detection:**  
  Sysmon + ELK provided actionable visibility across all scenarios, especially when combined with custom Sigma rules.
- **Biggest Blind Spot:**  
  DNS tunneling could evade detection if queries were not sufficiently anomalous in length or structure; requires vigilant tuning.
- **Response Improvement:**  
  Timely log review and rule creation are critical for rapid detection and mitigation of sophisticated attacks.

---

## Recommendations

- Regularly update and tune SIEM detection rules, focusing on obfuscation and living-off-the-land techniques.
- Use layered monitoring (endpoint + network) for better coverage of stealthy APT activity.
- Schedule periodic red team simulations to validate detection and response capabilities.

---

## Appendix

- **Commands Used:** See individual scenario documentation.
- **Sigma Rules:** See `rules/` directory.
- **Raw Logs & Screenshots:** Available in `logs/` and `screenshots/` directories.
