---
categories:
  - "[[Interviews]]"
topics:
  - "[[SOC]]"
created:
---

**

### Spear Phishing URL Detected

- It is important to *NOT* visit the domain/URL in question from your production browser
- Refer to Company’s domain management account to determine whether the domain is owned by Company. If owned by Company, specify that in the incident Notes, close the investigation, and add the domain to Settings: Tagged Domains.
- Add any other “safe to ignore” domains to Settings: Ignored domains
- Add log data: search the “DNS Query” and “Web proxy Activity” log sets for the domain/IP during the incident time period. Note the following items wherever available, and add any relevant log data to the investigation:

– DNS nameserver IPs used for DNS queries
– User-Agent strings (client OS, browser version, etc)
– Specific URLs visited
– destination_ip
– geoip_organization
– geoip_country_name
– source_data - may sometimes contain a client’s local MAC address
- Perform IP/domain analysis using the “IP/domain analysis” steps above.
- Perform URL analysis using the “Website/URL analysis” steps above.
- If the activity is determined to be a false positive or if no further action is warranted, note your findings and close the ticket.
- If the activity is determined to be a true positive, escalate the incident in accordance with the Alert response and escalation process.

**
## Incident Response

At the most basic level preparation is the most important. Developing incident response plans, proper training, setting up tools (SIEM, EDR, communication). Creating playbooks

1. Identification
	1. Analyzing Indicators
		1. Review logs, alerts etc to determine if an incident has occurred
	2. Classify the incident
		1. Categorize the incident based on type (malware, insider threat, breach) and severity
	3. Document findings
		1. Record details about the incident, timeline, affected systems, potential impact
2. Containment
	1. Short-term Containment: taking immediate actions (disconnecting from network, blocking malicious IP's)
	2. Long-term containment: Applying patches or reconfiguring systems
3. Eradication
	1. Patching vulnerabilities and removing malware.
4. Recovery
	1. Restoring systems
	2. Validating security: testing systems to ensure they are functioning properly
	3. Monitor for recurrence

5. **Detection & Analysis**: I monitor alerts from SIEM, Defender, or IDS tools. I triage alerts using severity and indicators of compromise.
6. **Containment**: I isolate affected systems to prevent lateral movement, often using EDR, remove from network, or firewall policies.
7. **Eradication**: Remove malware, reset credentials, and apply patches.
8. **Recovery**: Reintroduce cleaned systems into the network with close monitoring.
9. **Post-Incident Review**: Conduct a root cause analysis, document lessons learned, and update playbooks."
---

### 🔹 2. **Diverse Client Base = Broad Exposure**

> “Because EisnerAmper works with clients across industries — finance, healthcare, tech, etc. — I’d have the chance to analyze a wide range of security incidents and threat landscapes. That variety is valuable to me as I continue developing as an analyst.”

### 🔹 4. **Learning & Growth Culture**

> “I’ve seen that EisnerAmper has a strong emphasis on employee development and cross-functional learning. Being able to learn from experienced professionals in audit, risk, and compliance — not just IT — would help me become a more well-rounded SOC analyst.”

### ✅ **What do you know about EisnerAmper?**

> “EisnerAmper is one of the largest accounting and advisory firms in the U.S., known for serving clients in industries like financial services, real estate, healthcare, and technology. Beyond its roots in audit and tax, the firm has made significant investments in advisory services — including risk, compliance, and cybersecurity.