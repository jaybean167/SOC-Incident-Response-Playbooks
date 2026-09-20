# Enterprise SOC Incident Response Playbooks

A collection of operational Incident Response (IR) playbooks and Standard Operating Procedures (SOPs) designed for SOC Tier 1/2 analysts. These playbooks provide structured workflows for detection, analysis, containment, eradication, and recovery, aligned with **NIST SP 800-61 Rev. 2** and mapped to the **MITRE ATT&CK** framework.

---

## Playbook Directory

| Playbook | Target Vector | MITRE ATT&CK Tactic / Technique | Primary Objective |
| :--- | :--- | :--- | :--- |
| [`phishing_response.md`](./playbooks/phishing_response.md) | Phishing / Malicious Email | Initial Access (T1566) | Triage reported emails, extract header indicators, block malicious domains, and purge inbox artifacts. |
| [`brute_force_ssh_rdp.md`](./playbooks/brute_force_ssh_rdp.md) | Password Spray / Brute Force | Credential Access (T1110) | Identify authentication anomalies, isolate offending IP addresses, and lock compromised user credentials. |
| [`malware_outbreak.md`](./playbooks/malware_outbreak.md) | Ransomware / Executable Malware | Execution (T1204), Impact (T1486) | Isolate affected endpoints from network, terminate malicious processes, and trace persistent artifacts. |

---

## Methodology Framework

All workflows in this repository follow the four phases of the NIST Incident Response Lifecycle:

1. **Preparation:** Establishing monitoring rules, baseline alerts, and communication channels.
2. **Detection & Analysis:** Alert validation, IOC extraction, scope determination, and threat classification.
3. **Containment, Eradication & Recovery:** Network isolation, credential revocation, artifact removal, and system restoration.
4. **Post-Incident Activity:** Post-mortem analysis, rule tuning, and incident report generation.
