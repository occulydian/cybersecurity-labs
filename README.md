# Cybersecurity Labs

This repository demonstrates hands-on cybersecurity skills through security monitoring, log analysis, identity and access management (IAM), and simulated attack scenarios using industry-standard tools and platforms.

## Lab Environment
- Custom-built virtual lab environment for controlled attack simulation and log analysis
- VirtualBox-based lab environment

### Network
- Host-only network (192.168.56.0/24)

### Systems
- Ubuntu Splunk server (log analysis platform) - 192.168.56.10
- Windows client with Sysmon + Forwarder (log source) - 192.168.56.20
- Kali Linux (attack simulation) - 192.168.56.30

### Tools Used
- Splunk Enterprise
- Sysmon
- Microsoft Entra ID
- VirtualBox

---

## Completed Labs

### Splunk Failed Login Detection
Detects brute-force login attempts using Windows Event Logs (Event ID 4625)

[View Lab](./splunk-failed-logins-lab)

![Sample Failed Login Detection](splunk-failed-logins-lab/images/failed_login_result.png)

### Sysmon Network Analysis
Detects port scanning activity using Sysmon Event ID 3 and Splunk

[View Lab](./sysmon-network-analysis-lab)

![Sample Network Analysis](sysmon-network-analysis-lab/images/EventID_3.png)

### PowerShell Process Investigation
Detects suspicious PowerShell execution using Sysmon Event ID 1 and command-line analysis

[View Lab](./powershell-process-investigation-lab)

![Sample Process Investigation](powershell-process-investigation-lab/images/compiled-events.png)

### Microsoft Entra ID Identity
Demonstrates user provisioning, RBAC, MFA, and audit logging in Microsoft Entra ID

[View Lab](./entra-id-identity-lab)

![Sample Entra Sign-In Log](entra-id-identity-lab/images/sign-in-log.png)

---

## Skills Demonstrated

### Security Monitoring and Detection
- Log ingestion and normalization (Splunk, Sysmon)
- SPL query development
- Identifying patterns indicative of malicious behavior
- Simulated attack scenarios (nmap reconnaissance, PowerShell abuse)
- Detection development using aggregation and filtering
- Process and network traffic analysis (Sysmon Event IDs 1 and 3)
- Detection of common attack techniques (brute-force, reconnaissance, obfuscation)

### Identity and Access Management (IAM)
- User provisioning and account management
- Security group administration
- Role-based access control (RBAC)
- Least-privilege access principles
- Multi-factor authentication (MFA) implementation
- Microsoft Entra sign-in and audit log review
- Authentication and access control monitoring

### Lab Development
- Virtual lab design and administration
- Security testing and validation
- Technical documentation and reporting
