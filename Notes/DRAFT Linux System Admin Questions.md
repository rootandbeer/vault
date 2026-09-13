---
categories:
  - "[[Interviews]]"
topics:
  - "[[Linux]]"
status:
created:
---

**1. How do you troubleshoot a Linux system that is experiencing performance issues?**

- **Answer:**  
    I start by identifying the root cause using tools like `top`, `htop`, or `vmstat` to monitor CPU, memory, and disk usage. If a specific process is consuming excessive resources, I investigate further with `ps` or `strace`. For disk-related issues, I use `iostat` or `df` to check disk I/O and free space. If the issue persists, I review system logs (`/var/log/syslog`, `dmesg`) for errors or warnings.

### Monitoring overall system performance
- `top`- system monitoring tool, displays info about the CPU, memory and process usage
- `htop`- visual interface for top
- `vmstat`- reports system statistics related to processes, memory paging, block i/o, and CPU activity
### Investigating specific processes
- `strace`- traces system calls and signals made by a process. ex. analyze overall system performance and identify bottlenecks
- `ps` information about running processes
### Disk related troubleshooting
- `iostat` - monitor disk i/o performance
- `df` - disk space usage
### System logs
- `/var/log/syslog`- System log that records messages from OS and apps
- `dmesg`- review of system and kernel logs


**2. How do you manage users and groups in Ubuntu?**

- **Answer:**  
    I use commands like `useradd`, `usermod`, and `userdel` to manage users, and `groupadd`, `groupmod`, and `groupdel` for groups. I also configure user permissions and sudo access by editing `/etc/sudoers` or adding users to the `sudo` group. For bulk user management, I use scripts or tools like `newusers` and `chpasswd`.
    

**3. How do you install and manage software on Ubuntu?**

- **Answer:**  
    I use `apt` for package management, including installing (`apt install`), updating (`apt update`), and removing (`apt remove`) software. For custom software, I may compile from source or use Snap/Flatpak. I also configure repositories in `/etc/apt/sources.list` and ensure dependencies are resolved.
    

---

### **End-User Support**

**4. How do you troubleshoot a printer that isn’t working on a Linux system?**

- **Answer:**  
    I start by checking if the printer is detected using `lpstat -p` or CUPS web interface. I verify the printer queue and restart the CUPS service if needed. If the issue persists, I check printer logs (`/var/log/cups/error_log`) and ensure drivers are installed. For network printers, I verify connectivity and configuration.

### **Summary**
1. Check physical connections, power, restart and check status
2. Ping the printer to ensure it’s reachable and port 9100 is open
	1. If unresponsive may have IP conflict → reconfigure printers network settings
	2. check DHCP lease table, if static make sure it is in correct subnet
	3. check firewall rules (`ufw` or `iptables`)
3. Check CUPS configuration and printer queue.
	1. Use `lpstat -p` to check printer state (idle, paused, offline)
	2. If paused or offline, re-enable using `cupsenable`
	3. if still offline remove & re-add in CUPS
4. Print a test page to confirm functionality.
5. Review logs for errors if issues persist.


**5. How do you resolve network connectivity issues for an end user?**

- **Answer:**  
    I use `ping` to check connectivity to the gateway and external sites. If there’s no connection, I verify the network interface status with `ip addr` or `ifconfig` and check DHCP with `dhclient`. I also inspect DNS settings (`/etc/resolv.conf`) and firewall rules (`ufw` or `iptables`). For VPN issues, I ensure the VPN client is configured correctly and review logs.
    
### **Networking Basics**

**8. How do you troubleshoot a DNS resolution issue?**

- **Answer:**  
    I use `nslookup` or `dig` to test DNS resolution. If resolution fails, I check `/etc/resolv.conf` for correct DNS servers and verify connectivity to those servers. I also inspect firewall rules to ensure DNS traffic (port 53) is allowed.

- `dig` - provides DNS record information... A records (IP for domains), MX records, NS records, CNAME (aliases)

**9. How do you configure and troubleshoot a VPN on Linux?**

- **Answer:**  
    I configure VPNs using tools like OpenVPN or `network-manager`. For troubleshooting, I check VPN logs, verify credentials, and ensure the correct ports are open. I also test connectivity to the VPN server and inspect routing tables (`ip route`) to ensure traffic is routed correctly.
    

### **User Experience Enhancements**

**10. How have you improved the usability of Linux for end users?**  
- **Answer:**  
I’ve customized desktop environments (e.g., GNOME, KDE) to simplify navigation and improve accessibility. I’ve also created documentation and training materials to help users transition to Linux. Additionally, I’ve resolved software compatibility issues by finding alternatives or configuring compatibility layers like Wine.

### **System Security & Maintenance**

**11. How do you ensure systems are secure and up to date?**  
- **Answer:**  
I regularly apply security updates using `apt` or `yum` and schedule automated updates with tools like `unattended-upgrades`. I also monitor for vulnerabilities using tools like Lynis or OpenSCAP and enforce security policies (e.g., password complexity, SSH key authentication).

**12. How do you handle patching for a large number of systems?**  
- **Answer:**  
I use automation tools like Ansible to apply patches across multiple systems simultaneously. I schedule maintenance windows to minimize downtime and test patches in a staging environment before deploying to production.

### **Collaboration & Documentation**

**13. How do you document troubleshooting steps and configurations?**  
- **Answer:**  
I maintain a knowledge base using tools like Confluence or Wiki.js. For each issue, I document the symptoms, steps taken, and resolution. I also version-control configuration files using Git and include comments for clarity.

**14. How do you collaborate with other teams to resolve issues?**  
- **Answer:**  
I communicate clearly and regularly with team members, using tools like Slack or email. For complex issues, I collaborate with developers, network engineers, or security teams, sharing logs and findings to reach a resolution quickly.

### **SLA & Resolution Time**

**15. How do you prioritize IT support requests?**  
- **Answer:**  
I prioritize based on urgency and impact. Critical issues (e.g., system downtime) are addressed immediately, while lower-priority tasks (e.g., feature requests) are scheduled accordingly. I use ticketing systems like Jira or Zendesk to track and manage requests.

**16. How do you ensure timely resolution of incidents?**  
- **Answer:**  
I follow SLA guidelines, setting clear expectations for resolution times. I escalate issues to senior staff or other teams when necessary and provide regular updates to stakeholders. I also conduct post-incident reviews to identify areas for improvement.

### **Bonus Questions**

**17. How do you handle a situation where a critical system goes down?**  
- **Answer:**  
I first assess the impact and notify stakeholders. I then isolate the issue by checking logs, network connectivity, and system resources. If the issue is beyond my expertise, I escalate it while continuing to assist. Once resolved, I document the incident and implement measures to prevent recurrence.