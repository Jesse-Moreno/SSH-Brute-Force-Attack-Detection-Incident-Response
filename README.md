# SSH-Brute-Force-Attack-Detection-Incident-Response

## Objective

A hands-on SOC cybersecurity project focused on detecting, investigating, and responding to simulated SSH brute-force attacks against a Linux server. Demonstrates practical skills in security log analysis, SIEM monitoring, threat detection, incident response, and system hardening.

### Skills Demonstrated

- Linux system administration and security configuration
- SSH authentication monitoring and security log analysis
- SIEM deployment, log ingestion, and event correlation (planned)
- Brute-force attack simulation and threat detection (planned)
- Detection rule development and alert investigation (planned)
- Incident triage, analysis, and response (planned)
- System hardening and access control (planned)
- Technical documentation and security reporting

### Tools & Technologies Used

- UTM: Virtual machine hosting and lab environment
- Ubuntu Linux: Target server and operating system
- OpenSSH: Secure remote access and authentication monitoring
- journalctl: System and SSH security log analysis
- grep: Log filtering and identification of relevant events
- SIEM platform (TBD): Centralized security monitoring and detection (planned)
- fail2ban: Automated blocking of repeated failed authentication attempts (planned)

## Steps

### Phase 1: Lab Environment Setup — Completed
- Installed UTM to create a virtualized cybersecurity lab.
- Deployed Ubuntu Linux as the target server.
- Configured SSH connectivity between the macOS host and Ubuntu VM.
- Verified successful remote access using SSH.
<img width="565" height="368" alt="P1-01-ubuntu-server" src="https://github.com/user-attachments/assets/936081d2-6e4c-4639-98ca-40538be0c8fc" />
<img width="566" height="509" alt="P1-02-ssh-connection" src="https://github.com/user-attachments/assets/381107ca-1304-44cd-babd-dc4114d6d5e4" />

  

### Phase 2: Security Logging — Completed
- Installed and configured OpenSSH Server.
- Verified SSH service operation.
- Examined SSH service logs using `journalctl`.
- Filtered relevant log entries using `grep`.

### Phase 3: SIEM Deployment — Pending

### Phase 4: SSH Brute-Force Simulation — Pending

### Phase 5: Detection Engineering — Pending

### Phase 6: Incident Investigation — Pending

### Phase 7: Incident Response & Hardening — Pending

### Phase 8: Final Documentation — Ongoing
