# Incident Response Playbook: Phishing & Malicious Email Triage

**Category:** Initial Access  
**Framework Mapping:** MITRE ATT&CK T1566 (Phishing)  
**Standard:** NIST SP 800-61 Rev. 2  

---

## 1. Detection & Identification

### Primary Trigger
* User report submitted via PhishAlarm / Secure Email Gateway (SEG) alert.
* SIEM rule alert indicating abnormal email attachment execution or suspicious link click.

### Initial Assessment Steps
1. **Header Analysis:** Extract and inspect raw email headers:
   * Verify `SPF`, `DKIM`, and `DMARC` validation status.
   * Compare `From:` address against `Return-Path:` for spoofing discrepancies.
   * Identify originating sender IP address and run reverse DNS lookup.
2. **URL / Attachment Triage:**
   * Extract embedded hyperlinks and analyze via URLScan.io / VirusTotal in an isolated sandbox.
   * Calculate SHA-256 hash of email attachments; verify against threat intelligence feeds.

---

## 2. Scope & Containment

### Immediate Containment
* **Network Isolation:** If user opened a malicious attachment or entered credentials, isolate endpoint via EDR agent.
* **Email Purge:** Request SEG / Exchange administrator purge of the email message across all tenant mailboxes using the Message-ID.
* **Network Blocking:** Add malicious domain/IP to perimeter firewall and web proxy blocklists.

### Account Protection
* Force password reset and revoke active session tokens for affected user accounts.
* Enable step-up Multi-Factor Authentication (MFA) check if credential harvesting is suspected.

---

## 3. Eradication & Recovery

1. **Endpoint Remediation:**
   * Run full EDR/AV scan on isolated host to remove secondary payloads.
   * Inspect persistence mechanisms (Scheduled Tasks, Registry Run Keys, Startup Folder).
2. **System Restoration:**
   * Restore endpoint to network once confirmed clean by security team.
   * Monitor user account activity for 72 hours for residual anomalies.

---

## 4. Post-Incident Activities

* **Documentation:** Log incident ticket details including timeline, affected users, IOCs, and MTTR metrics.
* **Detection Tuning:** Update SIEM rules or SEG keyword filters to prevent similar email vectors.
* **Awareness Training:** Assign targeted anti-phishing module to affected users if required by policy.
