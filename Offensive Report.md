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

`wpscan --url http://192.168.1.110/wordpress --wp-content-dir -at -eu`

1. Scanned the network to identify the IP addresses of Target 1.
`nmap 192.168.1.*`
![](/Images/nmapIP.png)

2. Identified exposed ports and services.
`nmap -sC -sV 192.168.1.110`
Target 1 has ports 22 (SSH), 80 (HTTP), and 111 (rcpbind) open.

![](/Images/nmap1.png)
![](/Images/nmap2.png)

3. Enumerated the WordPress site. One flag is discoverable after this step.
     - **Hint**: Look for the `Users` section in the output.
     - `wpscan --url http://192.168.1.110/wordpress --wp-content-dir -at -eu`

Results of our scan revealed 2 users, Michael and Steven.
![](/Images/WPUsers.png)

Hydra was used to uncover Michael's password, which was **michael**.

`hydra -l michael -P /usr/share/john/password.lst -vV 192.168.1.110 ssh`

![](/Images/MichaelPW.png)

4. Used SSH to gain a user shell for Michael on Target 1. 

`ssh michael@192.168.1.110`

![](/Images/SSHTarget1.png)

Flag 1: 
Found flag 1 in the html code for http://192.168.1.110/service.html

![](/Images/dirbuster.png)

![](/Images/flag1.png)

Flag 2:
Found flag 2 by digging through the file path for /var/www/ which hosts the source code for the website at 192.168.1.110

![](/Images/flag2.png)

5. Find the MySQL database password.
     - While looking for additional information, a config file called `wp-config.php` was found in `/var/www/html`. This contained credentials for a wordpress database.
![](/Images/db_creds.png)

During exploring the tables, flag 3 and 4 were found by dumping the `wp_posts` table.
Commands to access the flags are 

![](/Images/flag3.png)
![](/Images/flag4.png)

6. Use the credentials to log into MySQL and dump WordPress user password hashes.

7. Crack password hashes with `john`.
     - **Hint**: Start by creating a wp_hashes.txt with Steven and Michael's hashes, formatted as follows

      ```bash
      user1:$P$hashvalu3
      user2:$P$hashvalu3
      ```

8. Secure a user shell as the user whose password you cracked.

9. Escalate to `root`. One flag can be discovered after this step.
    - **Hint**:  Check sudo privileges. Is there a python command you can use to escalate to sudo?




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
