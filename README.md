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
<br>
<img width="565" height="368" alt="P1-01-ubuntu-server" src="https://github.com/user-attachments/assets/936081d2-6e4c-4639-98ca-40538be0c8fc" />

<p align="center"><em>Figure 1. Ubuntu Linux virtual machine configured in UTM, showing system information and network configuration.</em></p>
<div style="height: 8px;"></div>
<img width="566" height="509" alt="P1-02-ssh-connection" src="https://github.com/user-attachments/assets/381107ca-1304-44cd-babd-dc4114d6d5e4" />

<p align="center"><em>Figure 2. Successful SSH connection from macOS to the Ubuntu server, verifying remote access and authentication.</em></p>

### Phase 2: Security Logging — Completed
- Installed and configured OpenSSH Server.
- Verified SSH service operation.
- Examined SSH service logs using `journalctl`.
- Filtered relevant log entries using `grep`.
<img width="1042" height="113" alt="P2-03-ssh-authentication-logs" src="https://github.com/user-attachments/assets/e2d2d10b-ce9a-4355-a67e-a05751d1ec5f" />

<p align="center"><em>Figure 3. Successful SSH authentication events analyzed using journalctl, displaying accepted logins, session activity, timestamps, and source IP addresses.</em></p>

<img width="564" height="130" alt="P2-04-failed-ssh-login png " src="https://github.com/user-attachments/assets/1d47c3ed-6c7b-43a2-884a-1d10b48e82b3" />

<p align="center"><em>Figure 4. Failed SSH authentication attempts identified using journalctl, revealing unsuccessful login activity, timestamps, and source IP addresses.</em></p>

<img width="627" height="77" alt="P2-05-persistent-security-logs" src="https://github.com/user-attachments/assets/a45ac2bd-4a6d-4e09-9e15-48249e0b517f" />

<p align="center"><em>Figure 5. SSH authentication logs filtered using grep to isolate failed login attempts, demonstrating targeted security event analysis.</em></p>

### Phase 3: SIEM Deployment — Pending

### Phase 4: SSH Brute-Force Simulation — Pending

### Phase 5: Detection Engineering — Pending

### Phase 6: Incident Investigation — Pending

### Phase 7: Incident Response & Hardening — Pending

### Phase 8: Final Documentation — Ongoing
