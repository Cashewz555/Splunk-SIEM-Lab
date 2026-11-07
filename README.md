# Splunk-SIEM-Lab
"A home-made SOC Lab utilizing Splunk, Sysmon, and Windows 10 to monitor endpoint telemetry"
# Splunk & Sysmon Home SIEM Lab

## 🎯 Objective
The goal of this project was to build a functional Security Information and Event Management (SIEM) environment to practice log ingestion, endpoint monitoring, and threat hunting.

## 🏗️ Architecture
*   **SIEM/Host:** Splunk Enterprise (running on local Windows machine)
*   **Endpoint (Victim):** Windows 10 Virtual Machine (VirtualBox)
*   **Telemetry Agent:** Microsoft Sysmon (SwiftOnSecurity configuration)
*   **Log Shipper:** Splunk Universal Forwarder

## ⚙️ Configuration Steps
1.  **Sysmon Setup:** Installed Sysmon on the Windows 10 VM to capture detailed process creation and network events.
2.  **Forwarder Setup:** Installed the Splunk Universal Forwarder to send logs to the host machine on port `9997`.
3.  **Log Ingestion:** Created custom `inputs.conf` and `outputs.conf` files to specify the Sysmon Operational log channel and route the data to the Splunk indexer.
4.  **Service Permissions:** Modified the SplunkForwarder Windows Service to run as the Local System account to ensure it had the necessary permissions to read the Sysmon event logs.

## 🛠️ Challenges Overcame
*   **VirtualBox Networking:** Troubleshot network isolation issues by switching the VM's network adapter from NAT to Bridged to allow communication with the host machine.
*   **File Extension Issues:** Resolved issues with Windows hiding `.txt` extensions on configuration files, which prevented Splunk from parsing the inputs.
*   **Permissions:** Addressed Windows Service error 1053 by adjusting the logon account for the forwarder service.

## 🔍 Threat Hunting Example
To test the pipeline, I executed a baseline process (`calc.exe`) on the victim VM and used Splunk Processing Language (SPL) to hunt for the event:


```spl
index=main EventCode=1 Image="*calc.exe"
```


## Screenshot
<img width="590" height="413" alt="Screenshot 2026-11-07 052031" src="https://github.com/user-attachments/assets/749f311c-3b32-4213-95cb-545c2cb6c6fe" />
<img width="512" height="388" alt="Screenshot 2026-11-06 210221" src="https://github.com/user-attachments/assets/c87a317f-bbce-419d-9c9c-c1a8cb8f0b91" />
<img width="960" height="504" alt="Screenshot 2026-11-06 204555" src="https://github.com/user-attachments/assets/9c4744e3-a7c9-44da-82f1-46031018b746" />
