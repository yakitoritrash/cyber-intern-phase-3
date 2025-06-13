## Cyber Intern Phase 3 – Advanced Threat Hunting & APT Simulation

## Overview

This repository documents my experience in **Phase 3 of the Cybersecurity Internship Program**, focusing on simulating advanced persistent threat (APT) scenarios. The phase emphasizes realistic attack simulations, incident detection, and layered defense strategies to develop real-world threat hunting and response skills.

**Objective:**  
To emulate sophisticated cyber attacks, practice advanced detection and analysis using modern tools (Sysmon, ELK, Sigma, Zeek, etc.), and build a portfolio of incident response skills suitable for professional environments.

---

## Lab Setup

### 1. Lab Preparation

- **Windows Targets**: Enabled PowerShell Remoting, configured firewall for remote management.
- **Logging Tools**:  
  - **Sysmon** for deep system visibility.
  - **Winlogbeat** to forward logs to SIEM.
  - **ELK Stack** (Elasticsearch, Logstash, Kibana) for centralized log analysis.
- **Network Monitoring**:  
  - **Zeek** and **Suricata** for network anomaly detection.
- **Auxiliary Tools**:  
  - **Autoruns**, **Process Monitor** for persistence and process analysis.
  - **Wireshark** for packet inspection.

### 2. Repository Structure

```
cyber-intern-phase-3/
├── logs/
├── reports/
├── rules/
├── screenshots/
└── README.md
```

---

## Step-by-Step Simulation Guide

### Scenario 1: Fileless Malware with PowerShell

- **Simulation:**  
  Generated and executed a base64-encoded PowerShell payload simulating fileless malware via remote download.
- **Detection:**  
  - Queried Sysmon (Event ID 1) for suspicious PowerShell process launches.
  - Checked PowerShell transcript logs for encoded commands.
- **Validation:**  
  - Verified detection of encoded PowerShell payload and parent process anomalies.

### Scenario 2: Lateral Movement via RDP Brute Force

- **Simulation:**  
  Used Hydra to brute force RDP credentials from Kali Linux against Windows targets.
- **Detection:**  
  - Monitored Security logs for repeated Event ID 4625 (failed RDP logons).
- **Mitigation:**  
  - Enabled Windows account lockout policy after 5 failed attempts.

### Scenario 3: Persistence via Registry Run Keys

- **Simulation:**  
  Added a malicious persistence entry to `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run`.
- **Detection:**  
  - Queried Sysmon (Event ID 13) for suspicious registry modifications.
- **Cleanup:**  
  - Removed persistence keys after analysis.

### Scenario 4: Data Exfiltration via DNS Tunneling

- **Simulation:**  
  Exfiltrated base64-encoded data through crafted DNS queries.
- **Detection:**  
  - Used Zeek to monitor for unusually long or encoded DNS queries.
  - Created and tested a Sigma rule (`rules/apt_dns_tunneling.yml`) for DNS exfil patterns.

### Scenario 5: Credential Dumping and Exfiltration

- **Simulation:**  
  Simulated credential dumping by listing LSASS and exfiltrating fake credentials over HTTPS.
- **Detection:**  
  - Monitored Sysmon (Event ID 10) for suspicious access to LSASS.
  - Checked SIEM for HTTP POST requests to suspicious domains.

---

## Post-Simulation Actions

### 1. Timeline Analysis

- Generated event timelines from Security logs to reconstruct attack progression.

### 2. Rule Creation

- Authored custom Sigma rules for scenarios (e.g., DNS exfiltration) and validated them with sample data.

### 3. Cleanup

- Removed persistence mechanisms and cleared logs after exercise completion.

---

## Validation Checklist

- [x] PowerShell payload triggered Event ID 4104
- [x] RDP brute force attempts logged with Event ID 4625
- [x] Registry Run key modification captured (Sysmon Event ID 13)
- [x] Zeek logged DNS queries exceeding 100 characters
- [x] LSASS access detection alert generated

---

## Troubleshooting & Tips

- **Sysmon Service:**  
  Check status with `Get-Service sysmon`
- **Winlogbeat Connectivity:**  
  Validate with `Test-NetConnection <ELK_IP> -Port 5044`
- **Real-Time Protection:**  
  Re-enable with `Set-MpPreference -DisableRealtimeMonitoring $false`
- **General Advice:**  
  - Replace all IP addresses and domains with internal lab equivalents.
  - Run each scenario sequentially, leaving 15–30 minutes between simulations for clear log separation.
  - Use SIEM dashboards and Kibana queries to correlate multi-stage attacks.

---

## Documentation Template

```
# APT Simulation Report - [Date]

## 1. Fileless Malware
- **Log Evidence**:  
  ![Sysmon Event ID 1](screenshots/fileless_sysmon.png)

## 2. RDP Lateral Movement
- **Hydra Command**:  
  ```bash
  hydra -L users.txt -P passwords.txt rdp://192.168.1.50
  ```
  
[Repeat for all scenarios...]

## Lessons Learned
- Most effective detection method: [Your observation]
- Biggest blind spot: [Gap identified]
```

---

## Acknowledgements

- [Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Sigma Rules Project](https://github.com/SigmaHQ/sigma)
- [Elastic Stack](https://www.elastic.co/what-is/elk-stack)
- [Zeek Network Security Monitor](https://zeek.org/)
- MITRE ATT&CK Framework

---

This phase emulated nation-state APT techniques, strengthened threat correlation skills, and provided actionable, portfolio-ready content for real-world cybersecurity roles. cyber-intern-phase-3
