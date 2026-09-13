---
categories:
  - "[[Interviews]]"
topics:
  - "[[Networking]]"
created:
---

### 🔐 **Domain Cybersecurity**

**Q1: How would you protect an organization's domain from phishing attacks?**  
**A:**

- Implement SPF, DKIM, and DMARC records to prevent email spoofing.
- Use email filtering to flag or quarantine suspicious messages.
- Provide regular phishing awareness training to staff.
- Monitor domain reputation and use threat intelligence tools.

**Q2: What’s your process for conducting a domain security assessment?**  
**A:**

- Use vulnerability scanners to detect misconfigurations or outdated services.
	- Outdated public facing services
	- Open ports that shouldn't be
	- Missing HTTP security headers
	- Misconfigured firewalls or ACL's
- Review DNS settings, SSL certificates, and domain registration lock.
	- DNS
		- Confirm SPF, DKIM, DMARC are correctly configured
		- Remove unused domains and wildcards
	- SSL/TLS
		- Ensure certificates are not explired
		- use TLS 1.2 or 1.3
- Check for active phishing or typo-squatting domains.
	- Use dnstwister.report, urlscan.io, phishtank and search for "like domains"
- Audit login activity and set up alerts for anomalies.
	- Setup alerts for:
		- Logins from foreign countries
		- Unusual login times
		- Multiple failed logins
		- Users logging in from unknown browsers or devices
	- How to enforce:
		- Enable 2FA
		- Alert on OAuth token abuse or abnormal API activity
		- Integrate a SIEM

---

### 📱 **Mobile Device Management (MDM)**

**Q3: What are some key policies you would enforce with an MDM solution?**  
**A:**

- Require device encryption and screen lock.
- Enforce remote wipe capabilities for lost/stolen devices.
- Install only approved apps (via allowlist).
- Disable USB data transfer and camera when not needed.

**Q4: How would you monitor mobile devices for potential threats?**  
**A:**

- Use MDM tools (like Intune or Jamf) to get alerts on jailbroken/rooted devices.
- Check for unusual network activity or unauthorized app installs.
- Monitor compliance dashboards for policy violations.
	- Device Encryption
	- Passcode Policy (weak passwords, no password etc)
	- Outdated OS
	- App Compliance
	- firewall/antivirus
	- BYOD Policies

---

### 🧰 **Google Workspace Cybersecurity**

**Q5: How do you implement access control in Google Workspace?**  
**A:**

- Use organizational units (OUs) to assign role-based access.
- Enforce 2FA for all users.
- Set up context-aware access (e.g., block logins from unknown locations/devices).
- Regularly audit user permissions and group memberships.

**Q6: What steps would you take to prevent data leaks in Google Workspace?**  
**A:**

- Enable DLP rules to detect and block sensitive content (SSNs, credit cards).
- Restrict sharing outside the organization.
- Monitor audit logs for abnormal sharing behavior.
- Educate users on safe sharing practices.

---

### 🎥 **Network Security Cameras**

**Q7: How do you ensure the security of network-connected cameras?**  
**A:**

- Change default credentials and limit access using VLANs/firewall rules.
- Keep firmware up to date and disable unused services (e.g., Telnet).
- Use secure protocols (HTTPS, RTSP over TLS).
- Log and review access to camera systems.

**Q8: What would you check if a security camera is offline?**  
**A:**

- Verify power and network connectivity (PoE switch or adapter).
- Check if the IP is reachable and if DNS is resolving.
- Review logs for recent updates or restarts.
- Restart the camera and ensure it's assigned a static IP or DHCP reservation.

---

### 🧠 **General/Behavioral Technical Questions**

**Q9: Describe a time you worked with a team to solve a security issue.**  
_Be ready with an example involving collaboration, issue resolution, and postmortem lessons._

**Q10: How do you stay current with cybersecurity trends and threats?**  
**A:**

- Subscribe to threat intel sources like CISA alerts, BleepingComputer, and Hacker News.
- Participate in cybersecurity communities (e.g., Reddit, Discord, LinkedIn groups).
- Take part in trainings, webinars, or CTFs to stay hands-on.


## Questions

“I noticed the position is listed as fully onsite — could you share a bit more about what drives that requirement? I’m curious if there are specific aspects of the role that make hybrid or remote work challenging, or if it’s something the organization has considered.”

how big is the company?

how many people are in this dept?

with the wide scope of  this role, how does five keys foresee the responsibilities being prioritized?

**WORK ENVIRONMENT**  

- Moderate to high noise level
- Work environment varies from site to site
- Some personal safety risks working in underserved communities
- Must be willing and able to work in a correctional facility, travel throughout the area to community sites, and travel state-wide to various locations or meetings as needed
- Up to 30% travel to Five Keys locations throughout California as needed