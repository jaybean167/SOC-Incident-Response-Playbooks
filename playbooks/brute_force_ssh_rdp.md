# Incident Response Playbook: SSH & RDP Password Spray / Brute Force Triage

**Category:** Credential Access  
**Framework Mapping:** MITRE ATT&CK T1110 (Brute Force)  
**Standard:** NIST SP 800-61 Rev. 2  

---

## 1. Detection & Identification

### Primary Trigger
* SIEM threshold alert triggering on excessive failed login attempts (e.g., >50 failed SSH/RDP authentications within 5 minutes).
* EDR alert flagging suspicious process execution following repeated authentication failures (`sshd`, `lsass.exe`).

### Initial Assessment Steps
1. **Authentication Log Parsing:**
   * Extract target username, source IP, failure count, and timestamps from SIEM/syslog (`/var/log/auth.log` or Windows Event ID `4625`).
   * Determine whether the attack is targeting a single user (Brute Force) or multiple users using common passwords (Password Spray).
2. **IP Reputation & Geolocation Verification:**
   * Query source IP address against threat intelligence platforms (AbuseIPDB, VirusTotal, GreyNoise).
   * Check if the source IP originates from an unauthorized geolocation or known TOR exit node.

---

## 2. Scope & Containment

### Immediate Containment
* **Network Isolation / IP Blocking:**
  * Add the offending source IP address to perimeter firewall / host firewall (`ufw` / Windows Firewall) drop rules.
  * Adjust Fail2ban / EDR dynamic blocking thresholds to auto-drop connection attempts.
* **Account Lockdown:**
  * If a successful login occurred (`Event ID 4624` or SSH `Accepted password`), immediately lock the affected user account.
  * Revoke active Kerberos tickets and terminate active SSH/RDP sessions.

---

## 3. Eradication & Recovery

1. **Compromise Audit:**
   * Audit system commands executed during the window of unauthorized access (check `.bash_history` or Process Command Line logs).
   * Scan for persistent backdoor accounts or newly added authorized SSH keys (`~/.ssh/authorized_keys`).
2. **Credential Remediation:**
   * Enforce mandatory password reset for all target account credentials.
   * Verify Multi-Factor Authentication (MFA) enforcement across all remote access entry points.

---

## 4. Post-Incident Activities

* **SIEM Rule Tuning:** Update correlation rules to trigger dynamic rate-limiting or automated firewall drop actions upon threshold breach.
* **Documentation:** Log incident details, containment duration, source IPs, and compromised credentials into the ticketing system.
