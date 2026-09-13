---
categories:
  - "[[Security]]"
type: Scoring
domain: "Reporting & Triage"
topics:
  - "[[XSS]]"
status: Complete
platform: []
severity:
cwe: CWE-79
owasp:
tools: []
created:
updated: 2025-07-16
---




**a. Attack Vector (AV)**
- XSS is typically exploitable via the network (e.g., through a web browser).
	- AV:N (Network)
	
b. **Attack Complexity (AC)**
- XSS is usually straightforward to exploit if the vulnerability exists.
	- AC:L (Low)
	
c. **Privileges Required (PR)**
- XSS often requires no privileges to exploit.
	- PR:N (None)

**d. User Interaction (UI)**
- XSS usually requires user interaction (e.g., clicking a malicious link).
	- UI:R (Required)
	
e. **Scope (S)**
- XSS typically affects only the vulnerable application (not other components).
	- S:U (Unchanged)
	
f. **Impact Metrics**
- Confidentiality (C): XSS can lead to data theft (e.g., session cookies).
	- C:H (High)
- Integrity (I): XSS can allow an attacker to modify content.
	- C:H (High)
- Availability (A): XSS usually does not affect availability.
	- A:N (None)

**Final CVSS Vector:**
> CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N


## Examples:

### **Reflected XSS**
- **Description**: The payload is reflected back to the user (e.g., via a URL parameter).
- **Impact**: Typically affects only the user who interacts with the malicious link.
- **Base Metrics**:
    - **Attack Vector (AV)**: Network (**AV:N** )
    - **Attack Complexity (AC)**: Low (**AC:L** )
    - **Privileges Required (PR)**: None (**PR:N** )
    - **User Interaction (UI)**: Required (**UI:R** )
    - **Scope (S)**: Unchanged (**S:U** )
    - **Confidentiality (C)**: High (**C:H** ) – Can steal session cookies or sensitive data.
    - **Integrity (I)**: High (**I:H** ) – Can modify content.
    - **Availability (A)**: None (**A:N** ) – No impact on availability.

### **Stored XSS**
- **Description**: The payload is stored on the server (e.g., in a database) and affects multiple users.
- **Impact**: Affects all users who access the compromised resource (e.g., a comment section).
- **Base Metrics**:
    - **Attack Vector (AV)**: Network (**AV:N** )
    - **Attack Complexity (AC)**: Low (**AC:L** )
    - **Privileges Required (PR)**: None (**PR:N** )
    - **User Interaction (UI)**: None (**UI:N** ) – No interaction required for exploitation.
    - **Scope (S)**: Unchanged (**S:U** )
    - **Confidentiality (C)**: High (**C:H** ) – Can steal session cookies or sensitive data.
    - **Integrity (I)**: High (**I:H** ) – Can modify content.
    - **Availability (A)**: None (**A:N** ) – No impact on availability.

### **Reflected XSS in a Banking Application (Critical Application)**
- **Description**: Reflected XSS in a banking application could allow an attacker to steal sensitive financial data or session cookies.
- **Environmental Metrics**:
    - **Confidentiality (C)**: High (**C:H** ) – Financial data is highly sensitive.
    - **Integrity (I)**: High (**I:H** ) – Modifying transactions or account details is critical.
    - **Availability (A)**: None (**A:N** ) – No impact on availability.
    - **Security Requirements**:
        - **Confidentiality Requirement (CR)**: High (**CR:H** )
        - **Integrity Requirement (IR)**: High (**IR:H** )
        - **Availability Requirement (AR)**: Medium (**AR:M** ) – Banking systems need high availability.

### **Stored XSS In a Banking Application (Critical Application)**
- **Description**: Stored XSS in a banking application could affect all users accessing a compromised resource (e.g., a transaction history page).
- **Environmental Metrics**:
    - **Confidentiality (C)**: High (**C:H** ) – Financial data is highly sensitive.
    - **Integrity (I)**: High (**I:H** ) – Modifying transactions or account details is critical.
    - **Availability (A)**: None (**A:N** ) – No impact on availability.
    - **Security Requirements**:
        - **Confidentiality Requirement (CR)**: High (**CR:H** )
        - **Integrity Requirement (IR)**: High (**IR:H** )
        - **Availability Requirement (AR)**: Medium (**AR:M** ) – Banking systems need high availability.


---
### Supporting Content
- https://www.first.org/cvss/calculator/3.1