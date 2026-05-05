# PowerShell Process Investigation Lab

## Objective
- Identify and investigate suspicious endpoint processes

---

## Environment

### Network

- Host-only network (192.168.56.0/24)

### Systems

- Ubuntu Server (Splunk Enterprise): 192.168.56.10
- Windows 11 Client (WINDOWS_LAB, log source): 192.168.56.20
- Kali Linux (attacker simulation): 192.168.56.30


### Software

- Splunk Enterprise (log analysis platform)
- Splunk Universal Forwarder (log forwarding to Splunk on port 9997)
- Sysmon (enhanced Windows event logging)

Remote access methods:
- Commands: ssh occulydian@127.0.0.1 -p 2222
- Protocol: SSH (TCP 2222)
- Authentication: username/password

Splunk Access:
- Web interface: http://127.0.0.1:8000
- Backend server IP: 192.168.56.10
- Access method: Localhost port forwarding / loopback interface
- Protocol: HTTP

---

## Data Collection

- Verified Sysmon Event ID 1 (process creation) logging in PowerShell

Used the query
```spl
index=main EventID=1
| table _time host Image ParentImage CommandLine User
| sort - _time
```
- Confirmed that EventID 1 logs are appearing in Splunk

- Ran the following commands
```cmd
cmd.exe /c powershell.exe -nop -c "whoami"
```
- Created a parent-child relationship, which is commonly used in attack chains

```PowerShell
powershell -nop -enc dwBoAG8AYQBtAGkA
```
- Executed an encoded PowerShell command, which is suspicious due to the command being obfuscated


```PowerShell
powershell -ExecutionPolicy Bypass -Command "Get-Process"
```
- Simulated an execution policy bypass, which can allow scripts to run even when system policies would normally restrict them


---

### Detection Queries

```spl
index=main EventID=1 Image="*powershell.exe"
NOT ParentImage="*splunkd.exe"
| table _time host ParentImage CommandLine User
| sort - _time
```

- Identifies unusual PowerShell commands that were generated, filtering out benign processes to more easily observe the more relevant activity

```spl
index=main EventID=1 ParentImage="*cmd.exe" Image="*powershell.exe"
| table _time host ParentImage Image CommandLine
| sort - _time
```

- Detects executed parent-child processes. This may indicate command nesting, which is commonly used to obfuscate execution flow and evade detection


```spl
index=main EventID=1 CommandLine="*-enc*"
| table _time host Image CommandLine
| sort - _time
```

- Identifies encoded PowerShell commands, which are commonly used to obfuscate payloads and evade detection

```spl
index=main EventID=1 Image="*powershell.exe"
| stats count by ParentImage CommandLine
| sort - count
```

- Compiles the PowerShell events to better examine a recurring execution patterns. This would be helpful for identifying suspicious behavior and building an attack timeline

---

### Key Takeaways

- Investigated endpoint activity using Sysmon process creation logs (Event ID 1)
- Identified suspicious PowerShell execution patterns
- Parent-child process relationships are critical for identifying living-off-the-land techniques commonly used in attacks
- Analyzed command-line arguments to detect obfuscated commands
- Observed techniques that align with common attacker behaviors such as activity obfuscation and command chaining
- Demonstrated the ability to differentiate between benign and suspicious process behavior

---


### Next Steps

- Correlate process creation events with network connections (Event ID 3) and authentication logs to build an attack timeline
- Expand detection queries to search for additional suspicious command-line arguments
- Convert detection queries into Splunk alerts for real-time monitoring

---

## Screenshots

![Live Data Ingestion in Splunk](live-data-ingestion.png)
![Event ID 1 Output in Windows Host](eventID1-output.png)
![Event ID 1 in Splunk](eventID1-splunk.png)
![Identification of Unusual PowerShell Usage](powershell-usage-identification.png)
![Identification of Parent-Child Processes](parent-child-identification.png)
![Identification of Encoded Command Arguments](encoded-command-identification.png)
![Compiled Suspicious Events in Splunk](compiled-events.png)
