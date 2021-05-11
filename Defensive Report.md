# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets

### Network Topology

The following machines were identified on the network:
- ELK
  - **Operating System**: Ubuntu 18.04.4 LTS
  - **Purpose**: To monitor network activity this machine holds the Kibana dashboards.
  - **IP Address**: 192.168.1.100

- Kali
  - **Operating System**: Linux 5.4.0-kali3-amd64
  - **Purpose**: Used for pentesting of the vulnerable WordPress server.
  - **IP Address**: 192.168.1.90

- Capstone
  - **Operating System**: Ubuntu 18.04.1 LTS
  - **Purpose**: This machine has Filebeat and Metricbeat installed. These will forward logs to the ELK machine.
  - **IP Address**: 192.168.1.105
  
- Target 1
  - **Operating System**: Linux 3.16.0-6-amd64
  - **Purpose**: This machine exposes a vulnerable WordPress server.
  - **IP Address**: 192.168.1.110


### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors
Excessive HTTP Errors is implemented as follows:
  - **Metric**: Count the number of login attempts and alert when there are more than 400 of the same type in the last 5 minutes. 
  - **Threshold**: Alert is triggered it 400 of the same type HTTP request is made in the previous 5 minute span.
  - **Vulnerability Mitigated**: HTTP Flood Attack
  - **Reliability**: High reliability

#### HTTP Request Size Monitor
HTTP Request Size Monitor is implemented as follows:
  - **Metric**: Sums the total number of bytes that have been requested over the last minute.
  - **Threshold**: Alert is triggered if 3500 bytes or more of data is requested in the previous minute.
  - **Vulnerability Mitigated**: Performance issues related to increased traffic volume to and from a server.
  - **Reliability**: High reliability

#### CPU Usage Monitor
CPU Usage Monitor is implemented as follows:
  - **Metric**: Monitors the total amount of CPU in use.
  - **Threshold**: Alert is triggered when 50% or more of the total CPU system resources are in use.
  - **Vulnerability Mitigated**: Resource hogging by cryptomining bots or other malicious programs that are CPU-intensive.
  - **Reliability**: High reliability
