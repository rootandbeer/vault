---
categories:
  - "[[Security]]"
type: Scoring
domain: "Reporting & Triage"
topics:
  - "[[CSRF]]"
status: Complete
platform: []
severity:
cwe: CWE-352
owasp:
tools: []
created:
updated: 2025-07-16
---


#### **a. Attack Vector (AV)**
- **Score:** Network (AV:N )
- **Reason:** CSRF exploits typically occur over a network, where an attacker tricks a user into making an unintended request.
#### **b. Attack Complexity (AC)**
- **Score:** Low (AC:L )
- **Reason:** Exploiting CSRF usually requires minimal complexity. The attacker only needs to craft a malicious request and trick the victim into executing it (e.g., via a malicious link or website).
#### **c. Privileges Required (PR)**
- **Score:** Low (PR:L ) or None (PR:N )
- **Reason:** If the victim has privileges to perform the action (e.g., change account settings, transfer funds), the attacker can exploit CSRF without needing additional privileges. If no privileges are required, score it as None.
#### **d. User Interaction (UI)**
- **Score:** Required (UI:R )
- **Reason:** CSRF requires the victim to interact with the malicious request (e.g., clicking a link or visiting a malicious site).
#### **e. Scope (S)**
- **Score:** Unchanged (S:U )
- **Reason:** CSRF typically does not escalate privileges or affect other systems beyond the targeted application.
#### **f. Confidentiality (C), Integrity (I), Availability (A)**
- **Confidentiality (C):** None (C:N )
    - **Reason:** CSRF does not directly compromise confidentiality.
- **Integrity (I):** High (I:H )
    - **Reason:** CSRF allows an attacker to perform unauthorized actions on behalf of the victim, compromising data integrity.
- **Availability (A):** None (A:N )
    - **Reason:** CSRF does not typically impact the availability of the system.


