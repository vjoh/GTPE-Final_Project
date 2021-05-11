# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -sC -sV 192.168.1.110
```
![nmap results](/Images/nmap1.png)
![nmap results](/Images/nmap2.png)

This scan identifies the services below as potential points of entry:
- Target 1
  - Port 22 / SSH
  - Port 80 / http
  - Port 111 / rpcbind

Nikto was used to conduct a basic vulnerability scan against 192.168.1.110 and find tracked critical vulnerabilities with OSVDB (Open Source Vulnerability Database). 
While OSVDB is no longer supported, the signatures can be cross checked with ![https://cve.mitre.org/data/refs/refmap/source-OSVDB.html](https://cve.mitre.org/data/refs/refmap/source-OSVDB.html).

The following vulnerabilities were identified on Target 1:
  - List of
  - Critical
  - Vulnerabilities

![Nikto](/Images/nikto1.png)


### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_sudo
