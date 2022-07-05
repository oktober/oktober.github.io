---
layout: post
title: OWASP Top Ten - Sensitive Data Exposure
tags: tech
description: 
thumbnail: /3-sensitive-data-exposure/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@pankajpatel
thumbnail-creator: Pankaj Patel
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: List of data
date: 2020-12-02
---

If you remember some of the more high-profile data breaches, such as the Equifax one in 2017 or the Target one in 2013, then you're familiar with sensitive data exposure.

Granted, it's not the only example of it, just a common one we hear about.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## How Data Exposure Happens
[According to OWASP](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure.html){:target="_blank"}, sensitive data exposure happens when malicious actors attack you in the following ways:
- Stealing your private keys
- Performing MITM *(Monster-In-The-Middle)* attacks (where they steal data over the wire)
- Breaking into a server (like a database) and stealing its data
- Performing MITB *(Monster-In-The-Browser)* attacks (where they steal decrypted data once it's received by the browser)

---

## Possible Ways To Help Prevent Sensitive Data Exposure
Like most vulnerabilities, this is a complex one to defend against.

You could encrypt everything, but what if an attacker gets into your server, steals your data, then decrypts it on their own time?

Or what if you use a strong encryption algorithm only to find out later it has a serious flaw or was incorrectly implemented?

Like everything with security, you're going to need to do a lot of research to find the best option for your situation.

A good place to start is OWASP's recommendations for [how to prevent sensitive data exposure](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure.html){:target="_blank"}. 

At a minimum, they recommend to:
- **Classify data processed, stored or transmitted by an application**. Identify which data is sensitive according to privacy laws, regulatory requirements, or business needs.
- **Apply controls** as per the classification.
- **Don't store sensitive data unnecessarily**. Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. ***Data that is not retained cannot be stolen***.
- Make sure to **encrypt all sensitive data at rest**.
- Ensure **up-to-date and strong standard algorithms**, protocols, and keys are in place; use proper key management.
- **Encrypt all data in transit** with secure protocols such as TLS with perfect forward secrecy (PFS) ciphers, cipher prioritization by the server, and secure parameters. Enforce encryption using directives like HTTP Strict Transport Security ([HSTS](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html){:target="_blank"}).
- **Disable caching for response that contain sensitive data**.
- **Store passwords using strong adaptive and salted hashing functions with a work factor** (delay factor), such as [Argon2](https://www.cryptolux.org/index.php/Argon2){:target="_blank"}, [scrypt](https://wikipedia.org/wiki/Scrypt){:target="_blank"}, [bcrypt](https://wikipedia.org/wiki/Bcrypt){:target="_blank"}, or [PBKDF2](https://wikipedia.org/wiki/PBKDF2){:target="_blank"}.
- **Verify independently the effectiveness of configuration and settings**.

*(**Bold** emphasis mine)*

I also recommend reading [OWASP's Top Ten Proactive Controls 2018 - C8: Protect Data Everywhere](https://owasp.org/www-project-proactive-controls/v3/en/c8-protect-data-everywhere){:target="_blank"}. 

Some things they recommend are:
- Only the minimum data should be stored on a mobile device *(they are regularly lost or stolen)*. All sensitive data should be kept in the secure keystore such as Android keystore or iOS keychain.
- For key storage:
	- Ensure the secret key is protected from unauthorized access
	- Store keys in a proper secrets vault
	- Use independent keys when multiple keys are required
	- Build support for changing algorithms and keys when needed
	- Build application features to handle a key rotation
- For application secrets management
	- Don't store secrets in code, config files, or pass them through environment variables
	- Keep keys and other application-level secrets in a secrets vault

These are just some highlights. You should definitely [read through the whole thing](https://owasp.org/www-project-proactive-controls/v3/en/c8-protect-data-everywhere){:target="_blank"} yourself.

---

## Sum It Up
Sensitive data exposure is a huge problem.

As web developers, it's our duty to build secure applications that protect data while it's in use, in transit, and at rest.

But data exposure risks don't stop there. We also have to think about what data we're collecting, if we even should be collecting it, and how we'll protect it. We have to think about key management and using strong encryption.

**There's a lot of factors that go into keeping data safe**. It's not an easy problem to solve and requires lots of study and practice. 

Check out the resources above and the ones below to keep learning more about this ever-present security vulnerability.

---

#### Further Reading
- [OWASP's HSTS Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html){:target="_blank"}
- [OWASP's Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html){:target="_blank"}
- [NIST's Cryptographic Algorithm Validation Program](https://csrc.nist.gov/Projects/Cryptographic-Algorithm-Validation-Program){:target="_blank"}
- [OWASP's iOS Testing Guide](https://github.com/OWASP/owasp-mstg#ios-testing-guide){:target="_blank"}
- [OWASP's Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html){:target="_blank"}
- [OWASP's Testing for Weak SSL/TLS Ciphers (from the Web Security Testing Guide)](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_SSL_TLS_Ciphers_Insufficient_Transport_Layer_Protection){:target="_blank"}
- [OWASP's Transport Layer Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html){:target="_blank"}
- [OWASP's User Privacy Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html){:target="_blank"}