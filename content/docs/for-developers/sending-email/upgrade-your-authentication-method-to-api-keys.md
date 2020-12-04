---
seo:
  title: Upgrade your authentication method to API keys
  description: How to upgrade your authentication methods in your code to use API keys
  keywords: Authentication, authorization, API key
title: Upgrade your authentication method to API keys
group: sending-email
weight: 0
layout: page
navigation:
  show: true
---

<call-out type="warning">

Two-Factor Authentication is required as of Q4 2020, and all Twilio SendGrid API endpoints will reject new API requests and SMTP configurations made with a username and password via Basic Authentication.

</call-out>

## Am I Using Basic Authentication with Username and Password?

- If SMTP: Check the username you're using to authenticate. If it's the word "apikey" then you're using an API Key. If it's not, then you're using username and password authentication.
- If V3: If you're using the most recent version of our open sourced libraries and followed the documentation, you should already be using API keys. If you aren't using our libraries, check the Authorization header on your API calls. If you see the word "Bearer" - you're good. If you see the word "Basic" you're using username and password.
- If V2 non-mail send: You're using username and passwords.
- If Premier 50M and below package: Use the [Email Activity API](https://sendgrid.api-docs.io/v3.0/email-activity/) to identify mail/send API Key usage. If the `api_key_id` parameter is empty, you will need to upgrade your authentication.

## Why should I use API Keys for authentication?

Using your account username and password for authentication is less secure than using an API Key. API Keys are preferred because you can limit permissions for API Keys and revoke them at any time. We recommend applying the principle of least privilege, using limited API Key permission to only provide access to what is needed for that request for maximum security.

For more about protecting your account, see our blog post, [7 Best Practices to Protect your Twilio SendGrid Account and Sending Reputation](https://sendgrid.com/blog/7-best-practices-to-protect-your-twilo-sendgrid-account-and-sending-reputation/). To learn more about the principle of least privelege, see our [Twilio blog post](https://www.twilio.com/blog/principle-of-least-privilege-details-best-practices) and [OWASP's article on Access Control](https://owasp.org/www-community/Access_Control).

## Tips for upgrading to API Keys

- Prioritize upgrading your authentication method to API Keys for your mail/send endpoints first
- Store your API Keys in an [environment variable]({{root_url}}/ui/account-and-settings/api-keys/#storing-an-api-key-in-an-environment-variable)
- Test these changes in a staging environment before rolling out to your production environment

## Upgrade to API Keys for your API endpoint requests

Follow these steps to identify and replace your authentication method to API Keys and then implement 2FA for enhanced security.

1. Identify where you are storing your username and password credentials for basic authentication. Your credentials could be stored in environment variables or hard coded depending on how you integrated.
1. Generate API Keys in the SendGrid UI or programmatically with the least privileged permissions required for each of the endpoints you will be updating. For more information, see [API Key Permissions]({{root_url}}/ui/account-and-settings/api-keys/#api-key-permissions).
1. Replace your username and password credentials with API Keys.

- For v3 API: Follow [these instructions](https://sendgrid.api-docs.io/v3.0/how-to-use-the-sendgrid-v3-api/api-authentication).
- For v2: Follow [these instructions](https://www.twilio.com/docs/sendgrid/api/v2/using_the_web_api#authentication).

## Upgrade to API Keys for your SMTP integration

1. Generate API Keys in the SendGrid UI or programmatically with the least privileged permissions required for each of the endpoints you will be updating. For more information, see [API Key Permissions]({{root_url}}/ui/account-and-settings/api-keys/#api-key-permissions).
1. Replace your username and password credentials with the newly-converted API key. You will want to use a converted version of your username for SMTP integration as well. Everyone's username is `apikey`.  <<include example here, language is confusing>>

Once the previous steps are completed, enable Two-Factor Authentication for all of your users, including [subusers]({{root_url}}/ui/account-and-settings/subusers/) and [teammates]({{root_url}}/ui/account-and-settings/teammates/).

## Check for API rejections

After enabling 2FA monitor your API calls for rejections related to continued use of username and password authentication.

### Error message for API calls:

`invalid authentication method` - declined because you are using basic authentication with 2FA enabled. to fix, update to using an API key or disable 2FA and switch to using IP Access Management for security. for more information, see https://sendgrid.com/docs/for-developers/sending-email/authentication/#basic-authentication

### Error message for SMTP:

SMTP error message is: `535 Authentication failed: Basic authentication is not allowed with 2FA enabled`. To fix, see https://sendgrid.com/docs/for-developers/sending-email/authentication/#basic-authentication

For more information on how to secure your Twilio SendGrid account visit this [blog post](https://sendgrid.com/blog/7-best-practices-to-protect-your-twilo-sendgrid-account-and-sending-reputation/).
