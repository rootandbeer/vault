---
categories:
  - "[[Interviews]]"
topics:
  - "[[SOC]]"
created:
---



**Dealing w/ False Positives**
- Understand the context of the alert
	- look at the source of the alert
		- Is it a begnine process or user?
		- what events and activities does it correlate with?
- Tuning the detection rules
	- make sure siem is finely tuned and not overly broad
	- adjust thresholds of the alert
- Document the false positive and solution, this will help find patterns and provide faster solutions if it comes up again

Siem takes data from various sources to identify threats in real time. Helps to identify threats in real time

****How to triage and Prioritize incidents****

- Assess the severity of the incident based on potential impact on business, sensitivity of data involved, likelihood of lateral movement
- High visibility incidents like customer facing systems or executive accounts are prioritized

****MIRE ATT&CK Framework****

- Knowledgebase of adversary tactics and techniques.
- Covers the entire attack lifecycle from gaining access to exfiltration
- example: I used it to find gaps in our monitoring and noticed we didn't have any rules in place for Unsecured Credentials which i found over 2500 of them. This resulted in creating a playbook on remediation and scanning of secrets in code.

****How to defend against insider threats****

- Use a combination of technical controls, monitoring and employee awareness.
- Implement the principle of least privilege.
- Monitory employee account behavior for unusual activity

****Endpoint Detection & Response (EDR)****

- provides visibility into endpoint activities and enabling fast detection and containment of threats.
- Monitor endpoints for suspicious behavior
- - file modifications
    - large downloads
    - unusual process execution
- Can automatically respond to threats by isolating the endpoint or terminating processes

## Incident Response

At the most basic level preparation is the most important. Developing incident response plans, proper training, setting up tools (SIEM, EDR, communication). Creating playbooks

1. Identification
2. Analyzing Indicators
    1. Review logs, alerts etc to determine if an incident has occurred
    2. Classify the incident
    3. Categorize the incident based on type (malware, insider threat, breach) and severity
    4. Document findings
    5. Record details about the incident, timeline, affected systems, potential impact
3. Containment
	1. Short-term Containment: taking immediate actions (disconnecting from network, blocking malicious IP's)
    1. Long-term containment: Applying patches or reconfiguring systems
5. Eradication
	1. Patching vulnerabilities and removing malware.
	2. Recovery
6. Restoring systems
    1. Validating security: testing systems to ensure they are functioning properly
    2. Monitor for recurrence

  

>[!example] Example Scenario: Ransomware Attack  
>1. ****Preparation:**** The organization has an IRP, a trained team, and tools like EDR and backups in place.  
>2. ****Identification:**** The SIEM alerts on unusual file encryption activity, and the team confirms it’s ransomware.  
>3. ****Containment:**** The affected systems are isolated from the network, and malicious processes are terminated.  
>4. ****Eradication:**** The ransomware is removed, and vulnerabilities (e.g., unpatched software) are fixed.  
>5. ****Recovery:**** Systems are restored from backups, and functionality is validated.  
>6. ****Lessons Learned:**** The team reviews the incident, updates playbooks, and implements additional email filtering to prevent future phishing attacks.

  

>[!TLDR] Example Scenario: Phishing  
>1. ****Preparation:**** Employees have undergone security awareness training. The organization has deployed email filtering tools (mimecast, proofpoint) and EDR  
>2. ****Identification:**** Employee reports suspicious email to security team. EDR Tool detects unusual activity on employees workstation, like outbound connection to known malicious IP  
>3. ****Containment:**** Immediately disable compromised account. Employee workstation is isolated from the network. Phishing email is quarantined across all mailboxes. Malicious link are blacked at the firewall and email gateway  
>4. ****Eradication:**** Compromised account password is reset, enforce MFA. Phishing email domain is added to blocklist  
>5. ****Recovery:**** Employees workstation is cleaned and reconnected. Send company wide email reminding them about suspicious emails.

## Deploying Splunk
**Configure indexer and search head**
- **Indexer:** responsible for storing and indexing the data. Configured for log collection, data parsing and indexing
- **Search Head:** Allows users to query the indexed data
**Configure Data Inputs**
- Define data sources (log files, syslog, network events) that you want splunk to ingest
- Set up forwarders on devices to send data to the main splunk server or indexer