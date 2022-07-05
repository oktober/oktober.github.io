---
layout: post
title: OWASP Top Ten - Broken Authentication
tags: tech
description: 
thumbnail: /2-broken-authentication/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@_mukhalchukfoto
thumbnail-creator: Pasha Mukhalchuk
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Open lock hanging off an old door with a bolt latch
date: 2020-11-18
---

Authentication is the backbone to any web application. Without it, we'd have web "sites", but probably no web "applications" - or at least only very insecure ones.

Logging in allows users to interact with our sites. It expands what they can do and enables us to move our businesses completely online. With web applications users can shop, check their email, write books, pay their bills, and so much more.

**All of this is possible because of authentication.**

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

Authentication means our users can create a unique account in our application. They can use our site and not worry about others messing with their stuff - at least that's the hope.

The second vulnerability in the OWASP Top 10 deals with this very problem - Broken Authentication.

When there are **problems with account creation, recovery, and proper use - you have some form of broken authentication** going on.

There are two use cases for web developers when it comes to authentication - building proper authentication for users of your app and following secure authentication practices when using 3rd party software.

Let's dive into the first scenario, then briefly discuss the second.

---

## Users Authenticating To Use Your App
Are you wondering if your app might have vulnerabilities due to broken authentication?

[OWASP has some excellent guidelines](https://owasp.org/www-project-top-ten/2017/A2_2017-Broken_Authentication){:target="_blank"} of common ["code smells"](https://en.wikipedia.org/wiki/Code_smell){:target="_blank"} for broken authentication.

**Does your app...**
- permit automated [credential stuffing](https://en.wikipedia.org/wiki/Credential_stuffing){:target="_blank"} or [brute force](https://en.wikipedia.org/wiki/Brute-force_attack){:target="_blank"} attacks?
- allow users to set default, weak, well-known, or breached passwords?
- use weak or ineffective credential recovery and forgotten password processes *(like "knowledge-based answers" which cannot be made safe)*?
- use plain text or weakly hashed passwords?
- have missing or ineffective MFA *(Multi-Factor Authentication)*?
- expose session IDs in the URL?
- not rotate session IDs after successful login?
- not properly invalidate session IDs?

If you have any of these or other authentication-related vulnerabilities, **what can you do?**

There are **at least 7 areas of your application you need to analyze** for authentication issues:
1. [HTTPS - everywhere](#https-everywhere)
2. [Registering new accounts](#registering-new-accounts)
3. [Logging in](#logging-in)
4. [Session management](#session-management)
5. [Ongoing account maintenance](#ongoing-account-maintenance)
6. [Forgotten passwords](#forgotten-passwords)
7. [API security](#api-security)

Let's talk about each of these.

---

<a name="https-everywhere"></a>
### 1. HTTPS - Everywhere
First, we start with the base level - always using HTTPS.

When it comes to authentication, [MITM *(Monster-In-The-Middle)* attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack){:target="_blank"} are a very real and dangerous possibility. HTTPS ensures your traffic is safe from prying eyes and manipulation across the wire.

Always use HTTPS in your web app - everywhere. Yes, everywhere.

If you have unsecured portions of your website (such as images, scripts, etc), these can be used to steal improperly secured session cookies. This can allow an attacker complete access to a user's account without even having to deal with passwords and MFA.

To help prevent these and other harmful attacks, **use HTTPS *everywhere* in your application**.

---

<a name="registering-new-accounts"></a>
### 2. Registering New Accounts
The next step is evaluating our enrollment process for new accounts.

#### Usernames
To make it easy for everyone, **usernames should be case-insensitive**.

If a user enters 'ZombieHunter999' for their username, they should be able to login as:
- zombiehunter999
- ZoMbIeHuNtEr999
- ZOMBIEHUNTER999
or using any other combination of capital and lowercase letters.

This makes it easy for users who might not remember which letters are capitalized or lowercase. It also helps you to store and retrieve the data.

Before saving a new username in your database, **force it to either all lower or all capital letters first**.

#### Passwords
You want to make it easy for new users to register and for them to create a secure account.

You can do this by requiring "strong" passwords.

What makes a password "strong"? [According to NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecret){:target="_blank"}, you should:
- Require passwords to be at least 8 characters long
	- Though it could be a good idea to enforce a larger minimum length *(e.g. 12 characters minimum)*
- Require them to be no more [than 64 characters long](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#maximum-password-lengths){:target="_blank"}
	- Don't truncate the password - just warn users the 64 character limit has been reached
- Show users a password strength meter
- Allow [all printing ASCII characters *(including the space character)* and all Unicode characters](https://pages.nist.gov/800-63-3/sp800-63b.html#-5112-memorized-secret-verifiers){:target="_blank"}
- Compare their chosen password against lists of [breached passwords](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html#identifying-leaked-password){:target="_blank"}, dictionary words, commonly used passwords, and context-specific passwords *(something like the name of the provider or combining that with their username e.g. stacie@google)*
	- If a match is found, ask them to choose a different password and why they need to

#### MFA *(Multi-Factor Authentication)*
Given that, as of this writing, [there have been more than 10 billion accounts breached](https://haveibeenpwned.com/){:target="_blank"}, MFA should be a requirement for nearly every online account.

To begin, no, [security questions are NOT a valid form of MFA](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer.html){:target="_blank"}.

Also, [according to NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#-5131-out-of-band-authenticators){:target="_blank"},
> Methods that do not prove possession of a specific device, such as voice-over-IP (VOIP) or email, SHALL NOT be used for out-of-band authentication.

I personally disagree with them about the VOIP ban, but I understand their reasoning. Most people don't securely protect the accounts controlling VOIP numbers, but at least you can often secure them better than other accounts. Unfortunately, I can't secure my phone provider and prevent a [SIM Swap](https://en.wikipedia.org/wiki/SIM_swap_scam){:target="_blank"}.

Differences of opinion aside, when a user is registering a new account for your application, **you should require them to enable one of the following forms of MFA** *(listed in order of strength with **weakest first**)*:
- SMS/Text Message
- [OTP *(One-Time Password)*](https://en.wikipedia.org/wiki/One-time_password){:target="_blank"} - such as an authenticator mobile app
- [Cryptographic Device/Hardware Token](https://en.wikipedia.org/wiki/Security_token){:target="_blank"} - such as a [Yubikey](https://www.yubico.com/){:target="_blank"}

#### Password Storage
After the user creates their account, now it's up to you to securely store their password.

This could be an article itself, so I'm going to give you the highlights from [OWASP's Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html){:target="_blank"}, which **I HIGHLY recommend you read**.
- [Use bcrypt](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#bcrypt){:target="_blank"} unless you need to use PBKDF2 or understand how to properly use Argon2
- Set a reasonable work factor for your system *(for bcrypt this would be at least 12)*
- [Use a salt](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#salting){:target="_blank"} *(modern algorithms should do this for you automatically)*
- Consider [using a pepper](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#peppering){:target="_blank"} to provide an additional layer of security

---

<a name="logging-in"></a>
### 3. Logging In
After our users have registered, now they need to be able to log in... and we need to keep the malicious actors out. How can we do that?

#### How Malicious Actors Get In
First, we need to understand how the malicious actors are going to try and break into legitimate accounts.

Most often this is done with
- [Brute force](https://en.wikipedia.org/wiki/Brute-force_attack){:target="_blank"} - testing multiple passwords on a single account
- [Credential stuffing](https://en.wikipedia.org/wiki/Credential_stuffing){:target="_blank"} - testing username/password pairs from a breach on any number of other websites
- [Password spraying](https://www.microsoft.com/security/blog/2020/04/23/protecting-organization-password-spray-attacks/){:target="_blank"} - testing a weak password on many accounts

#### Generic Error Messages
Our first line of defense is **[using generic error messages](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#authentication-and-error-messages){:target="_blank"} when an incorrect username or password is entered**. This helps prevent malicious actors from guessing legitimate usernames and/or passwords more easily.

For example, if a username is guessed correctly but an incorrect password is used, the error message should be something generic such as: *"The user ID or password is incorrect"*

**Users, and malicious actors, shouldn't know if it was the username or password that was wrong.** You will lose a bit of user functionality, but it's generally worth it for the gain in security.

#### Avoid Timing Attacks
Another thing to watch out for is timing attacks. 

This is **when the code takes different amounts of time to return** depending on whether they've entered a:
- valid username/invalid password
- invalid username/valid password
- invalid username/invalid password

Like the error messages, we don't want to reveal anymore information than necessary. **Coding so our program returns in the *same amount of time* for all of the situations above** will help prevent malicious actors from discovering legitimate account credentials.

##### Further Reading
- [Testing for Timing Attacks](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing){:target="_blank"}

#### Possible Ways To Help Prevent Brute Force Attacks
If you've ever taken a look at your application's login logs, you'll know how bad of a problem brute forcing is.

Tons of lists of usernames and paswords are easily available for anyone interested in finding them. Many brute forcing tools exist that easily let anyone with minimal coding skills use those lists and try to brute force a login page - like [Wfuzz or THC Hydra]({% post_url 2020-06-23-Useful-Tool-for-Brute-Forcing-a-Login-Page %}).

Whether your users create strong passwords or not, **malicious actors will try to brute force your site**.

**How do we prevent it?** That's a great question and can only be answered by you. I recommend reading through OWASP's article on [Blocking Brute Force Attacks](https://owasp.org/www-community/controls/Blocking_Brute_Force_Attacks){:target="_blank"} and doing some more research of your own.

Brute force attacks are a huge problem right now and don't have an easy solution. There are different things you can try and hopefully you'll find a solution that works for your situation.

#### Possible Ways To Help Prevent Credential Stuffing and Password Spraying Attacks
Due to the massive amount of account breaches, **credential stuffing and password spraying are on the rise**. The best defense against both of these issues is [protecting user accounts with MFA](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html#multi-factor-authentication){:target="_blank"}.

Even if it's not feasible for you to require all your users to set up MFA, you can still enforce it behind the scenes. You can ask for additional information from the user **if you notice login attempts from the following** *([from OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html#multi-factor-authentication){:target="_blank"})*:
- a new browser/device or IP address
- an unusual country or location
- specific countries that are considered untrusted
- an IP address that appears on known blacklists *(as a VPN user I hate this one...)*
- an IP address that has tried to log in to multiple accounts
- a login attempt that appears to be scripted rather than manual

While not as good as MFA, you can [also offer some alternative options](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html#alternative-defenses){:target="_blank"}:
- secondary passwords, PINs, and security questions
- CAPTCHA
- IP badlisting *(previously known as "blacklisting")*
- device fingerprinting
- require unpredictable usernames

But be aware that these options will have security risks of their own.

There are more options you can try. I highly recommend reading through [OWASP's Credential Stuffing Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html){:target="_blank"} to learn more.

---

<a name="session-management"></a>
### 4. Session Management
Like brute forcing, session management requires a much bigger discussion than I can provide here, but **[OWASP offers some basic guidelines](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html){:target="_blank"}**.

Essentially, you want to make sure once a user logs in that they can stay logged in until they've completed what they need to do.

Part of this means **ensuring their session information is**:
- randomly created with high entropy
- securely shared with only them
- expired within a decent amount of time or on logout *(whichever is first)*

It's simple, but also very complicated to execute securely. I highly recommend you learn more about it. A great place to start is [OWASP's Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html){:target="_blank"}.

---

<a name="ongoing-account-maintenance"></a>
### 5. Ongoing Account Maintenance
This section is less technical, more administrative in nature, but just as important.

We've developed some harmful habits over the years when it comes to password security. One of those is requiring users to change their password every few months or so.

Granted, there are high-value systems that still need a process like this. But for most web applications, likely yours included, this process does more harm than good.

Passwords are already very difficult for the average user to use properly. **Requiring users to change their password regularly makes it less secure**. Instead of creating actually random and secure passwords, **users will only change them as much as they have to**.

For example, let's say a user has the password 'betyouwontguess930'. If they're required to change it in 3 months, they'll probably change it to something like 'betyouwontguess931'.

If a user has to make their password very different each time *(e.g. changing from 'betyouwontguess930' to 'dlwooodke403kd')*, they'll resort to writing passwords on sticky notes or other insecure forms of password storage.

**This is a well-known problem and makes regular password changes a poor, and possibly harmful, security measure**.

[According to NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecretver){:target="_blank"},
> Verifiers SHOULD NOT require memorized secrets to be changed arbitrarily (e.g., periodically). However, verifiers SHALL force a change if there is evidence of compromise of the authenticator.

This means **you shouldn't require a regular passwod change** *(unless PCI compliance mandates you to)*. The only time you should ask a user to change their password is if there's a data breach. At that point, you must require all users to change their passwords and tell them why. 

But if there's no data breach, **leave those passwords be**.

---

<a name="forgotten-passwords"></a>
### 6. Forgotten Passwords
Inevitably, users will legitimately forget or lose their passwords. To help with this, web applications need secure Password Reset methods.

Unfortunately, this is easier said than done. **Password Reset functionality often uses weaker authentication methods and is commonly targeted by malicious actors** to gain access to someone else's account.

When creating a Forgotten Password process, you'll need to consider two steps:
1. The Forgot Password request
2. How a user "proves" their identity

#### 1. Forgot Password Request
During the first step, you'll need to ask for the username/email to identify the account.

With this step, keep the following security measures in mind *(from [OWASP's Forgot Password Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html#forgot-password-request){:target="_blank"})*:
- return a consistent message for existent and non-existent accounts
- return responses in the same amount of time whether the acount exists or not *(to help prevent timing attacks)*
- prevent automated submissions with CAPTCHA, rate-limiting, etc
- use secure coding practices to prevent [SQL injection](https://en.wikipedia.org/wiki/SQL_injection){:target="_blank"}

#### 2. User "Proves" Their Identity
The next step involves one of many options, but inevitably requires the user to prove their identity using another form of verification.

Some possible options are:
- [URL tokens](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html#url-tokens){:target="_blank"}
- [PINs](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html#pins){:target="_blank"}
- [Offline methods](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html#offline-methods){:target="_blank"}
- [Security questions](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html#security-questions){:target="_blank"}

**Each option has its own security risks**. For example, if a malicious actor has taken over a user's email account, a URL with a token sent to that email address now gives the malicious actor full account access.

Be sure to do your research and determine which option(s) will work best for your situation and risk level.

Unfortunately we can't always require security from our users, but we can help them.

By encouraging alternate forms of authentication *(like attaching a phone number, additional email address, or sending the user backup codes when they first sign up)*, we can help users secure their accounts from malicious actors and also enable them to regain access if a password is forgotten.

**Don't forget alternative authentication avenues such as call centers, mobile sites, etc.** All processes to verify users should be [analyzed and tested for weaknesses](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel.html){:target="_blank"}.

---

<a name="api-security"></a>
### 7. API Security
One last area we need to consider when securing authentication is our APIs *(Application Programming Interfaces)*. We won't go into detail here, but want to remind you of the importance of analyzing their security.

**There are lost of pieces to consider with API security**. I highly recommend you read through [OWASP's API Security Project list of common vulnerabilities](https://owasp.org/www-project-api-security/){:target="_blank"}. It's still in the works, but is a great place to start your API security journey.

---

## Broken Developer Authentication
The second area we need to consider for broken authentication is with you - the developer.

No doubt you're using many 3rd party tools/servers/software to build and serve your applications.

**Remember to practice secure authentication with these** and any other tools you use.
- Always change default passwords
- Use strong passwords and store them securely *(like in a password manager)*
- Never store passwords in version control
- Implement MFA *(if and wherever you can)*

---

#### Further Reading
- [Understand your Authentication Assurance Level and how you should secure it](https://owasp.org/www-project-proactive-controls/v3/en/c6-digital-identity){:target="_blank"}
- [Selecting your AAL *(great visual flowchart)*](https://pages.nist.gov/800-63-3/sp800-63-3.html#-62-selecting-aal){:target="_blank"}
- [NIST's Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63-3.html){:target="_blank"} 
	- containing links to 3 other publications specifically for:
		- [Enrollment and Identity Proofing Requirements](https://pages.nist.gov/800-63-3/sp800-63a.html){:target="_blank"} 
		- [Authentication and Lifecycle Management](https://pages.nist.gov/800-63-3/sp800-63b.html){:target="_blank"} 
		- [Federation and Assertions](https://pages.nist.gov/800-63-3/sp800-63c.html){:target="_blank"} 