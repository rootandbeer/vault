---
categories:
  - "[[Labs]]"
topics:
  - "[[References/Azure]]"
created:
---


## Setup Slack SSO in Azure

- Enterprise Applications --> New Application --> slack
- Select SSO method: SAML --> edit
	- Contact slack support to get the correct URLS:
		- Add the Entity ID: `https://domain.slack.com`
		- Replay URL:  `https://domain.slack.com/sso/saml`
		- Sign On URL: `https://domain.slack.com`
- Download **Certificate (Base64)** on the **Set up single sign-on with SAML** page under **SAML Signing Certificate page**
- Copy the URLs on the **Set up Slack** section

## Configure Slack SSO

- In slack application go to **Settings & Administration --> Workspace Settings**
- Go to **Settings & Permissions** then select **Authentication** tab
- Select **Configure** at **SAML Authentication**
	- Paste **Login URL** in the **SAML SSO URL** form
	- Paste **Microsoft Entra Identifier** in **Identity provider issuer** form 
	- Copy contents from the downloaded certificate and paste into **Public Certificate** form.