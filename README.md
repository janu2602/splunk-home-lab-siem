# splunk-home-lab-siem

Splunk Enterprise SIEM home lab with Windows and Linux log ingestion, Sysmon integration, and security monitoring dashboard.

### Objective :

  The goal of this project was to establish a fully functional Security Information and Event Management (SIEM) environment. By deploying Splunk Enterprise on a Linux backend and integrating a Windows 10 endpoint, I created a pipeline to collect, index, and visualize real-time security telemetry. This lab simulates a production Security Operations Center (SOC) environment used for monitoring authentication events and system alerts.

### Architecture :

  The infrastructure utilizes a hub-and-spoke model where a central indexer receives encrypted traffic from remote sensors.
  
  1. Splunk Server : Ubuntu 24.04 VM running Splunk Enterprise as the "brain" for indexing and searching.
    
  2. Monitored Endpoint : Windows 10 VM acting as the "sensor," equipped with Sysmon for high-fidelity logging.
    
  3. Log Transport : Splunk Universal Forwarder (UF) installed on the Windows host to stream logs over port 9997.
    
  4. Network : Both VMs communicate over a dedicated VirtualBox NAT Network (10.0.2.0/24) to ensure internal connectivity and internet access for updates.

### Architecture Diagram :


<img width="1200" height="1300" alt="architecture-diagram" src="https://github.com/user-attachments/assets/4e94dac4-46f9-4494-8405-076bc2932dcd" />


### Skills Demonstrated :

  1. SIEM Deployment : Installation and configuration of Splunk Enterprise on Linux.
  
  2. Log Ingestion Pipeline : Setting up a receiving port and configuring a Universal Forwarder.
  
  3. Endpoint Telemetry : Implementing Sysmon with the SwiftOnSecurity configuration to capture process creation and network connections.
  
  4. SPL Proficiency : Writing Search Processing Language (SPL) queries to filter and aggregate security events.
  
  5. Dashboarding : Designing a "Security Monitoring" dashboard for real-time visualization of threats.

### Dashboard  :


The dashboard provides live visibility into failed logins, process executions, and outbound network traffic.


<img width="1262" height="801" alt="Screenshot 2026-02-28 185849" src="https://github.com/user-attachments/assets/2b5f4b4f-8c6a-4885-ad3b-98866b423b9e" />


## Configuration Files :


The following configurations were manually tuned on the Windows Universal Forwarder to define the data pipeline:


### inputs.conf

Defines the specific event channels (Security, System, Application, and Sysmon) to be collected.

[WinEventLog://Security]
index = main
disabled = false

[WinEventLog://System]
index = main
disabled = false

[WinEventLog://Application]
index = main
disabled = false

[monitor://C:\Windows\Sysmon64\Operational]
index = main
sourcetype = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
disabled = false

**outputs.conf**
Directs the forwarder to the Ubuntu Splunk Indexer on port 9997.

[tcpout]
defaultGroup = splunk_server

[tcpout:splunk_server]
server = [UBUNTU-VM-IP]:9997


