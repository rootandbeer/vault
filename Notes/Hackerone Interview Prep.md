---
categories:
  - "[[Interviews]]"
updated: 2025-07-16
---

## Interview Questions
1. **How to stay up with latest security vulnerabilities and trends**
	1. CISA - Known Exploited Vulnerabilities (KEV)
	2. OWASP top10
	3. Attend conferences and local meetups
	4. participate in CTF's
2. **How familiar are you with the CVSS scoring system, and how would you apply it to asses a vulnerability?**
	1. Considers exploitability, impact and required privileges. 
	2. First determine base score by evaluating attack vectors (internal or external)
	3. Consider the temporal and environmental scores by checking exploit availability and how critical the system is to the business.
3. **Explain how you would reproduce a reported vulnerability in a test environment?**
	1. Review the submitted to understand the attack and steps to reproduce
	2. Setup test environment using a VM, containerized environment or staging server
	3. Use the PoC in the report to verify if the vulnerability exists
4. **Walkthrough web application pentesting**
	1. Reconnaissance to gather information about the application like subdomains and endpoints
	2. Analyze API end points, user roles and authentication mechanisms using burpsuite
	3. Test for injection vulnerabilities (SQLi, XSS) by submitting payloads and checking responses
	4. Verify security headers (CSP, HSTS) are properly configured
	5. Document the process
	6. I map out user-controllable inputs and endpoints, by manually exploring the application to understand its functionality. Then, I use tools like Burp Suite to crawl the application and intercept HTTP requests. This helps me identify all the places where users can interact with the system, such as forms, URL parameters. I also inspect the HTML source code for hidden inputs. Once I've identified these inputs, I test them by sending various payloads to see how the application handles unexpected or malformed data. This process helps me uncover potential vulnerabilities like XSS, CSRF, or injection flaws.
5. **OWASP Top10**
	1. Access Control Vulnerabilities
	2. Price Oracle Manipulation
	3. Logic Errors
	4. Lack of Input Validation
	5. Reentrancy Attacks
6. **Burpsuite**
	1. Proxy: allows me to intercept and modify HTTP requests between the client and server
	2. Repeater: Use it to manually modify and resent requests to test for SQLi, XSS and authentication flaws.
	3. Intruder: Good for brute-forcing login credentials
	4. Extender: Used with 3rd party extensions
7. **What is the difference between XSS and CSRF?**
	1. XSS involves causing a website to return malicious JS
	2. CSRF involves inducing a victim to perform actions they do not intend to
8. **What is API?**
	1. Used in webapps / services
	2. Set of rules that allows software apps to communicate w/ each other
	3. allows access to information w/o having to know the internal workings of the system
9. **CVSS**
	1. **Base Metrics**
		- Exploitability
			- Attack Vector - remote, local, physical
			- Attack Complexity
			- Privileged Required to exploit
			- If user interaction is needed for exploit
		- Impact Metrics
			- Confidentiality - What the data is that the attacker has access to after infiltration
			- Integrity - If the data was tampered with or altered
			- Availability - can attacker deny the service to other users
		- Scope Metrics - Whether the vulnerability is in one system or across multiple systems	
	2. Temporal Metrics
		1. Exploit code maturity, is it theoretical, POC, or available
		2. Remediation availability
		3. Report Confidence - how confident that a vulnerability exists
	3. Environmental Metrics
		1. allows for modification of the base metrics

## How to Check for XSS and CSRF

I map out user-controllable inputs and endpoints, by manually exploring the application to understand its functionality. Then, I use tools like Burp Suite to crawl the application and intercept HTTP requests. This helps me identify all the places where users can interact with the system, such as forms, URL parameters, headers, and API endpoints. I also inspect the HTML source code for hidden inputs. Once I've identified these inputs, I test them by sending various payloads to see how the application handles unexpected or malformed data. This process helps me uncover potential vulnerabilities like XSS, CSRF, or injection flaws.

For XSS, I test for reflected, stored, and DOM-based vulnerabilities by injecting payloads and checking for improper sanitization.

For CSRF, I look for state-changing requests that lack proper anti-CSRF mechanisms and craft proof-of-concept exploits.


- Source: https://docs.hackerone.com/en/articles/8475055-report-components

The `Report ID#` and `title` can be found at the top of the page. This can be changed to match internal naming conventions.

The [[Report States|Report State]] is the color behind the `report number` and is represented by colors:

|            |                                                                       |
| ---------- | --------------------------------------------------------------------- |
| **Color**  | **Report State**                                                      |
| Purple     | New                                                                   |
| Light Blue | Needs more info                                                       |
| Dark Blue  | Pending Program Review _(currently in beta)_                          |
| Yellow     | [Retesting](https://docs.hackerone.com/en/articles/8520079-retesting) |
| Green      | Resolved                                                              |
| Orange     | Triaged                                                               |
| Brown      | Duplicate                                                             |
| Grey       | Informative                                                           |
| Red        | Not Applicable                                                        |

## Other sections from the source:

- **Triage Summary:** Triagers offer a clear and brief overview of the vulnerability to the customer.
- **Timeline:** Shows all activity that occurs in the report between the hackers and the program members involved with the triaging.

---

### Supporting Content:
- [[Report States]]
- [[Report Severity]]
