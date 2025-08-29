
# Endpoint Detection Tool (EDT) – Phase 1 Setup

This phase covers setting up the Endpoint Detection Tool environment from scratch on **AWS Windows instances**, installing and configuring **Sysmon** and **Wazuh agent**, and verifying everything is running properly.  

---

## 1. Windows RDP & Environment Preparation

- Launched **two Windows EC2 instances** in AWS.  
- Configured RDP access to connect remotely.  
- Opened necessary ports in AWS security groups for RDP and Wazuh agent communication.

> **AWS Security Notes:**  
> - RDP: TCP 3389 open for my IP only  
> - Wazuh agent: TCP 1514/1515 (agent-manager communication)  
> - Sysmon does not require open ports, runs locally  

---

## 2. Download & Install Sysmon

**Navigate to a temporary folder and download Sysmon:**

```powershell
cd $env:TEMP
Invoke-WebRequest -Uri "https://download.sysinternals.com/files/Sysmon.zip" -OutFile ".\Sysmon.zip"
Expand-Archive .\Sysmon.zip -DestinationPath .\Sysmon
```

**Download the SwiftOnSecurity Sysmon configuration:**

```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml" -OutFile ".\sysmonconfig.xml"
```

**Install Sysmon with the configuration and accept EULA:**

```powershell
cd .\Sysmon
.\sysmon64.exe -i ..\sysmonconfig.xml -accepteula
```

**Quick verification – check last 5 Sysmon events:**

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5 | Format-List TimeCreated,Id,Message -Force
```

![Sysmon Installation Verified](<PLACEHOLDER_FOR_SS>)

---

## 3. Install & Configure Wazuh Agent

**Edit the Wazuh configuration if needed:**

```powershell
notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"
```

**Check Wazuh service status:**

```powershell
Get-Service *wazuh*
```

Expected output:

```
Status   Name     DisplayName
------   ----     -----------
Stopped  WazuhSvc Wazuh
```

**Start the Wazuh agent:**

```powershell
Start-Service -Name WazuhSvc
Get-Service -Name WazuhSvc
```

Expected output:

```
Status   Name     DisplayName
------   ----     -----------
Running  WazuhSvc Wazuh
```

**Stop the Wazuh agent (if needed):**

```powershell
Stop-Service -Name WazuhSvc
```

![Wazuh Agent Running](<PLACEHOLDER_FOR_SS>)

---

## 4. Verification & Testing

- Ensured **Sysmon logs events** like process creation and registry changes.  
- Wazuh agent **connects to the manager successfully**.  
- Verified services start/stop correctly using PowerShell.  

**Sample Sysmon Events:**

```text
TimeCreated : 8/29/2025 1:09:58 AM
Id          : 1
Message     : Process Create:
              ProcessGuid: {…}
              Image: C:\Windows\System32\wbem\unsecapp.exe
              ParentImage: C:\Windows\System32\svchost.exe
```

![Sysmon Events Preview](<PLACEHOLDER_FOR_SS>)

---

## 5. Ports & Security Recap

| Service       | Port | Notes                                      |
|---------------|------|--------------------------------------------|
| RDP           | 3389 | Open only to my IP                          |
| Wazuh agent   | 1514 | Communication to Wazuh manager             |
| Wazuh agent   | 1515 | Secure connection (optional if manager uses SSL) |
| Sysmon        | N/A  | Local logging only                          |

---

### Phase 1 Summary

- AWS Windows instances spun up and configured.  
- Sysmon downloaded, configured, and installed using SwiftOnSecurity config.  
- Wazuh agent installed, configured, and verified.  
- All services tested: start/stop/status check ✅  
- Security groups configured to allow required ports.  
- System ready for Phase 2 (monitoring & alerting).

---

```
