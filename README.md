![image alt](splunk-white-black-bg.png)

# Splunk-lab

#  Splunk Windows Lab using VirtualBox

This lab simulates a small Windows environment to practice **log collection**, **SIEM analysis**, and **threat detection** using Splunk Enterprise.

##  Lab Goals
- Install and configure Splunk Enterprise on Windows
- Set up Windows clients to forward logs using Splunk Universal Forwarder
- Practice searching and analyzing Windows event logs
- Simulate real-world activity for detection testing

---

##  Lab Architecture


- VMs: 2 or more Windows machines
- Network: Bridged or Host-Only Adapter
- Log Type: Windows Event Logs (Security, System, Application)

---

##  Tools Used

| Tool                    | Purpose                          |
|-------------------------|----------------------------------|
| VirtualBox              | Virtualization                   |
| Windows Server (2019/2022) | Splunk Enterprise host         |
| Windows 10 or Server    | Log-generating clients           |
| Splunk Enterprise       | Log analysis & visualization     |
| Splunk Universal Forwarder | Log forwarding agent           |

---

##  Setup Instructions

### 1. Create VMs

- **Splunk Server**:
  - OS: Windows Server 2019/2022
  - RAM: 4GB+
  - Disk: 40GB
  - Name: `SplunkServer`

- **Client VM**:
  - OS: Windows 10 or Server
  - RAM: 2–3GB
  - Disk: 30GB
  - Name: `WinClient1`

### 2. Install Splunk Enterprise

- Download from [Splunk Downloads](https://www.splunk.com/en_us/download/splunk-enterprise.html)
- Use default install options
- Access web UI: `http://<splunk-server-ip>:8000`

### 3. Install Universal Forwarder on Clients

- Download from [Universal Forwarder Downloads](https://www.splunk.com/en_us/download/universal-forwarder.html)
- Set Splunk server IP and port `9997` during install
- Choose to monitor:
  - Security logs
  - Application/System logs

### 4. Enable Receiving on Splunk Server

- Go to: `Settings > Forwarding and Receiving > Configure Receiving`

<img src="2025-06-19 14_20_46-Window.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="2025-06-19 14_21_09-Window.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
2025-06-19 14_21_09-Window.png
  
- Enable TCP port `9997`

---

##  Basic Log Search Examples

```spl
index=* host=WinClient1
index=* sourcetype="WinEventLog:Security" EventCode=4625
index=* EventCode=4688
# Create a custom Application log
EventCreate /ID 100 /L APPLICATION /T INFORMATION /SO MyScript /D "Test Event from Lab"

# Trigger login failures
runas /user:wronguser notepad.exe
