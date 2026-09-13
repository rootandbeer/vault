---
categories:
  - "[[Security]]"
type: Technique
domain: Web AppSec
topics:
  - "[[XSS]]"
  - "[[CSRF]]"
status: Complete
platform:
  - Web
severity: Medium
cwe: CWE-352
owasp: "A01:2021 Broken Access Control"
tools:
  - Burp Suite
created:
updated: 2025-07-16
---


Three key conditions have to be present for attack to take place:

1. **Relevant Action:** An action within the application. i.e. Modifying permissions, changing user's own password etc..
2. **Cookie-base Session Handling:** Performing the action involves issuing one or more HTTP requests, and the application relies solely on session cookies to identify the user who made the request.
3. **No Unpredictable Request Parameters:** The request that performs the action does not contain parameters that has values an attacker cannot guess or determine. i.e. An attacker cannot change the users password if the attacker needs to know the value of the existing password.

When examining in `burpsuite` if its a `POST` request then a form will need to be created containing all of the form data. If it's a `GET` request then the information can be self-contained in the URL. I.E. www.vuln.com/change_email?email=youremail@changed.com.  

## Bypassing CSRF Tokens
### Testing Flaws in CSRF Tokens

There are several methods that can be used to potentially bypass CSRF tokens if they are not properly implemented. Several ways to test for improperly configured CSRF tokens:

1. Remove CSRF token and see if application accepts the request
2. Change the request method from `POST` to `GET`
3. See if CSRF token is tied to user session: Use your own CSRF token in place of victim's CSRF token. (Make sure to generate a new token as most of the time they can only be used once)

### Testing Flaws in CSRF Cookies
1. Check to see if the CSRF token is tied to CSRF cookie:
	- Modify and submit an invalid CSRF token
	- Submit a valid CSRF token from a another valid user
2. Submit a valid CSRF token and cookie from another valid user

>[!note] The CSRF Token and CSRF cookie are not the same. The CSRF cookie will be in the `cookie` line of the HTTP request.

### Exploiting CSRF Token Vulnerabilities
If returning successful requests from the above we can potentially 2 things:

1. Inject a csrfKey cookie in the user's session (HTTP Header Injection)
	1. Submit another form and see if there is another cookie parameter.
	2. If above is true, change the header i.e.: `GET /?search=test%20d%0aSet-Cookie:%20csrfKey=attacker_csrf_cookie HTTP/1.1` (change `search=test` to correct form parameters.)
2. Send a CSRF attack to the victim with a known CSRF token.
	1. In order to send the header injection request first we can use an image in our payload instead of using the tradational auto submit function: `<image src="http://site.com/?search=test%20d%0aSet-Cookie:%20csrfKey=attacker_csrf_cookie" onerror="document.forms[0].submit()"`
