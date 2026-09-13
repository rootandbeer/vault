---
categories:
  - "[[Interviews]]"
topics:
  - "[[IAM]]"
  - "[[References/Azure]]"
created:
---



**What is IAM?**
- framework of policies, technologies, and practices that makes sure the right individuals and systems have the appropriate access to resources.
- Covers lifecycle management, authentication (verifying identity), and authorization (granting access)
 on Markup Language)**: Used for exchanging authentication and authorization data between identity providers and servers (e.g. SSO)
- **OIDC (OpenID Connect)**: Built on top of OAuth 2.0, used for app authentication
- **OAuth 2.0**: Framework used for token-based access (delegating access to API's)

**User**
- a single person or service
- Can be assigned roles or policies directly or through group
**Group**
- a collection of users
- used to manage permission by assigning roles/policies to the group instead of individuals
**Role**
- a collection of permissions group together to represent a job function or access level
- assigned to users, groups, or services
**Policy**
- defines what permissions are allowed or denied
- attached to users, groups, or roles to control access
**Permission**
- Specific action that can be performed on a resource (read, write, delete, etc...)
- Permission building block used in roles and policies


**Examples**
- **Users**
	- **Employee** is software engineer, they should have access github, slack, and code repo
	- **Vendors** should be given limited access to staging
	- **Machines/Services** AWS function would be issued a temporary role to access S3 bucket
- **Groups**
	- **Engineering Team** should be granted access to internal engineering tools like terraform, aws, code repo,
	- **HR** would be restricted to workday and/or ADP
	- **Privileged Admin**
- **Roles / Permissions**


step up, mfa, sso
AD access