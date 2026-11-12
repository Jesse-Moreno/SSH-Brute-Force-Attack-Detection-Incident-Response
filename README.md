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

### Phase 3: SIEM Deployment — Completed
- Created an Elastic Security project on Elastic Cloud Serverless.
- Configured the System integration to collect Ubuntu logs from Journald using the `ubuntu-soc-lab` agent policy.
- Installed Elastic Agent on the Ubuntu VM and confirmed enrollment in Fleet.
- Opened a new SSH session from macOS to generate a successful login event.
- Searched the `logs-*` data view in Elastic Discover and verified the event’s timestamp, username, source IP, and outcome.
<img width="1438" height="821" alt="P3-06-ssh-event-in-siem" src="https://github.com/user-attachments/assets/fc32b333-debb-4d98-bc58-cfb406128a7a" />

<p align="center"><em>Figure 6. Elastic Discover shows a successful SSH password login for socadmin from 192.168.64.1 on ubuntu-soc-lab, with the event timestamp and Journald input visible.</em></p>

### Phase 4: SSH Brute-Force Simulation — Completed
#### Preliminary log validation

- Filtered the `logs-*` data view in Elastic Discover for `Failed password` events on `ubuntu-soc-lab`.
- Verified two historical SSH login failures for `socadmin` from `192.168.64.1`.
- Compared the timestamps and found that the failures occurred approximately 51 seconds apart.
<img width="1435" height="794" alt="P4-07-ssh-failed-logins-in-siem" src="https://github.com/user-attachments/assets/b5843b1b-ddb3-400b-84e3-0e4667f9b67d" />

<p align="center"><em>Figure 7. Elastic Discover shows two failed SSH password attempts for socadmin on ubuntu-soc-lab from 192.168.64.1, approximately 51 seconds apart.</em></p>

- Indexed six synthetic SSH authentication failures in Elasticsearch and tagged them `p4-synthetic`.
- Filtered for `p4-synthetic` in Elastic Discover and verified that five failures came from `192.0.2.10` within 40 seconds, with one comparison event from `192.0.2.20`.
<img width="1440" height="819" alt="P4-08-synthetic-ssh-failures-in-siem" src="https://github.com/user-attachments/assets/c3dcaa43-4edf-454a-9f16-47c24ae54e25" />

<p align="center"><em>Figure 8. Six synthetic SSH authentication failures displayed in Elastic Discover.</em></p>

### Phase 5: Detection Engineering — Completed
- Configured an Elastic Security threshold rule to flag five or more simulated SSH authentication failures from the same source IP in a single execution window.
- Validated the rule against the Phase 4 synthetic logs. The preview flagged `192.0.2.10` with five failures, while `192.0.2.20` stayed below the threshold with one.
- Enabled the rule and validated live alert generation using fresh synthetic events. The resulting alert identified source IP `192.0.2.10` with a threshold result count of five.
<img width="1440" height="819" alt="P5-09-ssh-threshold-rule-preview" src="https://github.com/user-attachments/assets/e430a1de-e2e8-4ee9-8e77-ba0f8fd31646" />

<p align="center"><em>Figure 9. Rule preview detecting five synthetic SSH authentication failures from 192.0.2.10.</em></p>

<img width="1440" height="819" alt="P5-10-ssh-threshold-alert" src="https://github.com/user-attachments/assets/de036ee3-19ff-4c11-95c1-557043137add" />

<p align="center"><em>Figure 10. Live threshold alert showing source IP 192.0.2.10 and five matching synthetic authentication failures.</em></p>

### Phase 6: Incident Investigation — Completed
### Investigation Findings

- Investigated the SSH threshold alert generated on November 10, 2026, at 16:58:15.944 Central.
- Identified five synthetic authentication failures from `192.0.2.10` targeting `socadmin` at approximately 16:57:59.
- Correlated the five source events with the alert's threshold result count of five.
- Searched for successful authentication events from the same source within the synthetic validation batch between 16:55 and 17:05 Central; no matching events were found.
- Classified the alert as an expected detection of authorized synthetic test activity.

### Incident Timeline

All times are Central on November 10, 2026.

| Time | Observed event |
|---|---|
| 16:57:59.506–16:57:59.511 | Five synthetic authentication failures recorded from `192.0.2.10`, targeting `socadmin`. |
| 16:58:15.944 | Elastic Security generated the SSH threshold alert with a matching event count of five. |
| Investigation window: 16:55–17:05 | Search returned no successful authentication events for the same source in the synthetic validation batch. |

### Alert Disposition

**Assessment:** Expected detection of authorized synthetic test activity.

The rule correctly identified the five matching failures. No successful authentication events were found within the scoped test batch and investigation window.

**Response decision:** No host containment was warranted for these directly inserted synthetic events. Ubuntu SSH hardening will be performed separately in Phase 7.

**Scope and limitations:** These events were inserted directly into Elasticsearch to validate detection and investigation workflows. They do not establish that real SSH attempts occurred or determine whether the Ubuntu host was compromised.
<img width="1440" height="821" alt="P6-11-alert-source-event-investigation" src="https://github.com/user-attachments/assets/f7576779-0bd9-4581-bfd4-d9da604ec0b2" />

<p align="center"><em>Figure 11. Investigation of five synthetic authentication failures from 192.0.2.10 targeting socadmin that matched the threshold alert.</em></p>

<img width="1440" height="820" alt="P6-12-successful-authentication-check" src="https://github.com/user-attachments/assets/874317ee-b925-4753-8c10-3268ff207c46" />

<p align="center"><em>Figure 12. No successful authentication events found for 192.0.2.10 in the synthetic validation batch during the selected investigation window.</em></p>

<img width="1440" height="818" alt="P6-13-investigation-note" src="https://github.com/user-attachments/assets/b3d4cb04-8906-4fbc-9608-4bbc8f919799" />

<p align="center"><em>Figure 13. Alert closed after investigation, showing source IP 192.0.2.10 and a threshold result count of five.</em></p>

<img width="1440" height="819" alt="P6-14-alert-closure" src="https://github.com/user-attachments/assets/bccffdad-493e-45de-b213-795e6603abc2" />

<p align="center"><em>Figure 14. Saved investigation note documenting event correlation, the successful-authentication search, authorized synthetic testing, and the investigation’s scope limitations.</em></p>

### Phase 7: Incident Response & Hardening — Completed
### Steps Taken
1. **Established SSH security baseline:** Inspected the effective OpenSSH configuration using `sshd -T`, confirming password authentication was enabled and public-key authentication was available (Figure 15).
2. **Generated an SSH key pair:** Created an Ed25519 public/private key pair on the macOS host and protected the private key with a passphrase.
3. **Authorized key-based access:** Added the public key to Ubuntu's `~/.ssh/authorized_keys` file and configured appropriate SSH directory and file permissions.
4. **Verified public-key authentication:** Successfully connected from macOS to Ubuntu using the private key and confirmed the authentication event in SSH logs.
5. **Disabled password authentication:** Created `/etc/ssh/sshd_config.d/01-soc-hardening.conf` with `PasswordAuthentication no`, `KbdInteractiveAuthentication no`, and `PubkeyAuthentication yes`.
6. **Validated and applied configuration:** Checked the configuration using `sshd -t`, verified effective authentication settings using `sshd -T`, and reloaded the SSH service.
7. **Tested password-based access:** Attempted an SSH connection with public-key authentication disabled. The server rejected the connection with `Permission denied (publickey)` (Figure 16).
8. **Confirmed authorized access remained functional:** Successfully reconnected to Ubuntu using the Ed25519 private key, verifying that SSH access remained available after hardening (Figure 17).

**Outcome:** Password-based SSH authentication was disabled while authorized public-key authentication remained functional, reducing exposure to password-based brute-force attacks.
<img width="515" height="368" alt="P7-15-ssh-security-baseline" src="https://github.com/user-attachments/assets/744e41d3-aad9-49a6-a522-d4f793c1db71" />

<p align="center"><em>Figure 15 — SSH Security Baseline: OpenSSH effective configuration before hardening, showing password authentication enabled, public-key authentication available, root password login restricted, and a maximum of six authentication attempts per connection.</em></p>

<img width="565" height="366" alt="P7-16-password-authentication-denied" src="https://github.com/user-attachments/assets/63aacdf6-8d2d-4810-a24e-575fcc63683e" />

<p align="center"><em>Figure 16 — SSH password authentication test showing "Permission denied (publickey)," confirming that password-based remote login is disabled after hardening.</em></p>

<img width="568" height="340" alt="P7-17-publickey-authentication-success" src="https://github.com/user-attachments/assets/d30f5f8e-83cf-4f3e-8a96-7af763f29cac" />

<p align="center"><em style="font-weight: normal;">Figure 17 — Successful SSH public-key authentication after hardening, confirming that authorized key-based access remains functional while password authentication is disabled.</em></p>

<img width="566" height="338" alt="P7-18-fail2ban-ssh-jail-status" src="https://github.com/user-attachments/assets/f188fa19-336c-451e-a5fd-040be42eade3" />

<p align="center"><em>Figure 18 — Fail2ban SSH jail successfully activated, monitoring SSH authentication events through the systemd journal with no failed attempts or banned IP addresses recorded at the time of verification.</em></p>

<img width="566" height="342" alt="P7-19-fail2ban-manual-ip-ban" src="https://github.com/user-attachments/assets/81243036-4ccc-4693-96b5-0968bba9db97" />

<p align="center"><em>Figure 19 — Controlled Fail2ban manual ban test showing the test IP address 192.0.2.10 successfully added to the SSH jail ban list, confirming that the jail accepts administrative ban commands.</em></p>

<img width="568" height="341" alt="P7-20-fail2ban-automatic-ban" src="https://github.com/user-attachments/assets/66ee8a86-459e-4d79-b7f2-c7a11601220c" />

<p align="center"><em>Figure 20 — Fail2ban automatically banned the loopback IP address 127.0.0.1 after five failed SSH authentication attempts, validating the configured detection threshold and automated blocking response.</em></p>

<img width="570" height="340" alt="P7-21-fail2ban-restoration" src="https://github.com/user-attachments/assets/c28cf3a5-6f27-4c2f-9805-110b62b053d9" />

<p align="center"><em>Figure 21 — Post-test Fail2ban restoration confirming successful removal of the temporary loopback IP ban, reactivation of self-IP protection, and zero currently banned addresses while retaining historical ban counters.</em></p>

<img width="570" height="340" alt="P7-22-fail2ban-persistence" src="https://github.com/user-attachments/assets/54c7de61-8bd1-426a-9c27-2efa56624a58" />

<p align="center"><em>Figure 22 — Successful Fail2ban service restart and SSH jail verification confirming that the persistent configuration retains the loopback and management-host exemptions, self-IP protection, and active SSH monitoring.</em></p>

### Phase 8: Final Documentation — Ongoing
