---
categories:
  - "[[Projects]]"
topics: []
created:
---


 typical pentest workflow (Enumeration → Information Gathering → Vulnerability Assessment → Exploitation → Advanced Techniques)

├── reconnaissance/            # Phase 1: Info gathering
│   ├── index.md
│   ├── passive-recon/
│   ├── osint/
│   └── dns-enumeration/
│
├── scanning/                  # Phase 2: Active discovery (keep existing)
│   ├── index.md
│   ├── host-discovery/
│   ├── port-scanning/
│   ├── service-enumeration/
│   ├── web-enumeration/ 
│   └── vulnerability-scanning/
│
├── exploitation/              # Phase 3: Initial access
│   ├── index.md
│   ├── web/
│   │   ├── index.md
│   │   ├── authentication/
│   │   ├── authorization/
│   │   ├── xss/
│   │   ├── sqli/
│   │   ├── ssrf/
│   │   ├── file-uploads/
│   │   └── command-injection/
│   ├── network/
│   │   ├── index.md
│   │   ├── smb/
│   │   ├── ldap/
│   │   ├── ftp/
│   │   ├── dns/
│   │   ├── snmp/
│   │   └── ssh/
│   └── binaries/
│       ├── index.md
│       ├── stack/
│       ├── heap/
│       └── rop/
│
├── post-exploitation/         # Phase 4: After initial access
│   ├── index.md
│   ├── enumeration/
│   ├── privilege-escalation/
│   │   ├── linux/
│   │   └── windows/
│   ├── lateral-movement/
│   ├── persistence/
│   ├── credential-dumping/
│   └── defense-evasion/
│
├── active-directory/          # Specialized: AD-specific
│   ├── index.md
│   ├── enumeration.md
│   ├── kerberos/
│   ├── delegation/
│   ├── trusts/
│   └── attacks/
│
├── cloud/                     # Specialized: Cloud platforms
│   ├── index.md
│   ├── aws/
│   ├── azure/
│   └── gcp/
│
├── wireless/                  # Specialized: Wireless
│   ├── index.md
│   ├── fundamentals/
│   ├── reconnaissance/
│   ├── attacks/
│   └── post-connection/
│
└── reference/                 # Quick reference & utilities
    ├── index.md
    ├── reverse-shells.md
    ├── tunneling.md
    ├── port-forwarding.md
    └── password-attacks.md