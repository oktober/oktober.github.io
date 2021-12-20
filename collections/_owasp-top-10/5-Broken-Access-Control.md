---
layout: post
title: OWASP Top Ten - 5. Broken Access Control
tags: tech
description: 
thumbnail: /5-broken-access-control/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@fritzkier
thumbnail-creator: Rizqi Handi
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Picture of chain link gate with holes ripped in it
date: 2021-01-08
---

If you've ever logged into your bank account or downloaded your tax forms online, you've dealt with access control.

If an attacker can access someone else's bank account, personal information, or download their tax forms, then you have a case of the #5 vulnerability - Broken Access Control.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

When developing restricted access to resources online (such as requiring a user to log in to access their account information), you must deal with at least 2 areas: **Authentication and Authorization**.

We discussed Broken Authentication *(failing to adequately verify a user is who they say they are)* in [a previous post]({{ site.baseurl }}{% link _owasp-top-10/2-Broken-Authentication.md %}).

In this post, we'll discuss the second, and just as complex, piece - **Broken Access Control *(aka Broken Authorization)***.

---

## What is Access Control?
In a nutshell, **access control is how you determine who gets access to what** *(e.g does an editor have access to create new users?)* **and how the code actually implements it**. How you control their access to resources = Access Control.

Access control is also known as authorization, but for simplicity we'll refer to it as access control in this post.

---

## Why Is Broken Access Control a Problem?
If you have access control vulnerabilities in your application, people could access resources they shouldn't be able to. Depending on the resource and/or person, that can be a very bad situation.

[According to OWASP](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control){:target="_blank"},
> "Failures *[of access control]* typically lead to unauthorized information disclosure, modification or destruction of all data, or performing a business function outside of the limits of the user."

**What does this mean?**

Well, let's talk about some possible situations where access control vulnerabilities exist:
- An S3 bucket is shared online without the proper access restrictions. Anyone with the link can access, view, and download the files in the bucket. This particular bucket contains loan applications for hundreds of applicants. **The information contained in those applications includes full names, dates of birth, social security numbers, and more**. An attacker stumbles across the link and downloads a copy of all the files. They then sell it on the black market resulting in identity theft and financial fraud for many victims and legal, financial, and reputation issues for the company who created the bucket.

- After login, an API queries user information and returns it with the path /id/638/profile. An attacker notices this pattern and, once they've authenticated, they make a call to /id/639/profile. **This returns the profile information for a completely different user**. Once this access control vulnerability is found, the attack iterates through all possible id's for users and steals their information.

- An attacker logs into a web application and notices a hidden link to an administration page. They try to browse to this link and **are able to access administration functionality, including modifying and deleting user accounts, even though they are only a regular-level user**. They use these features to compromise the application.

All of these situations have some form of broken access control that can have very damaging consequences. They may seem simplistic, but **many of these types of vulnerabilities (including more complex ones) still commonly occur in web applications**.

---

## How Do You Find Access Control Vulnerabilities?
**Most of the time broken access control is found through manually testing your application and resources.** Occasionally it can be found with SAST or DAST tools, but it is much more difficult that way.

Why? Because access control is about more than just code. It's about how that code really works and how users interact with it. The best way to discover these types of vulnerabilities is with a human using the application.

#### Further Reading
- [Testing for Broken Authorization *(Access Control)*](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/README){:target="_blank"}
- [OWASP's 2017 Top Ten #5 Broken Access control *(under "Is the Application Vulnerable?" section)*](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control){:target="_blank"}

---

## Possible Ways To Help Prevent Broken Access Control
Like authentication *(and just about everything in security)*, these vulnerabilities can be difficult to prevent. By implementing [OWASP's access control design principles](https://owasp.org/www-project-proactive-controls/v3/en/c7-enforce-access-controls){:target="_blank"} below, you can help strengthen the security of your application.

### 1. Design Access Control Thoroughly Up Front
It's much more difficult to change your access control after your application has been built. By deciding beforehand which access control model(s) you'll use and how you'll implement them, you can help reduce the likelihood of these types of vulnerabilities now, and in the future.

Some other tips to help with your access control design [from OWASP](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control){:target="_blank"}:
> - Utilize model access controls with record ownership
- Disable web server directory listings and ensure file metadata *(e.g. git)* and backup files are not present within web roots

### 2. Force All Requests to Go Through Access Control Checks
If every request goes through an adequate access control check, you are far less likely to accidentally give unauthorized users access to resources.

[OWASP](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control){:target="_blank"} also recommends you:
> - Use the same access control mechanisms throughout the application (and minimize CORS usage)
- Rate limit API and controller access to minimize the harm from automated attack tooling
- Invalidate JWT tokens on the server after logout

### 3. Deny By Default
This principle will help you catch access control edge cases. It's also known as "failing closed". If an error occurs, a feature isn't set set up, or something similar, then **it is coded to automatically deny access to it**.

### 4. Principle of Least Privilege
This is a basic, and very useful, security principle. It means you only give a user, program, process, etc the **bare minimum access it needs to do its job**. This will help reduce the chance they might get access to resources they shouldn't have access to.

### 5. Don't Hardcode Roles
Role-based access control *(e.g. giving access to a resource based on that user's role such as subscriber, editor, administrator)* is flexible, but prone to creating faulty access control, especially in large-scale applications.

Instead, **OWASP recommends using attribute or feature-based access control checks instead**. This helps code to scale while retaining access control customization capabilities.

### 6. Log All Access Control Events
Logging can help you identify an access control attack in progress or one that has already occurred. It's a valuable tool that will help you secure your application and/or identify if an attack has occurred.

According to [OWASP](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control){:target="_blank"}, you should:
> "Log access control failures, alert admins when appropriate"

### 7. Test for Access Control Failures
As we said in the beginning, access control can be difficult to identify. To help you find it before the attackers do, [OWASP recommends you](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control){:target="_blank"}:
> "include functional access control unit and integration tests"

#### Further Reading
- [Testing for Authorization](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/README){:target="_blank"}

---

## Sum It Up
Broken access control continues to be a common vulnerability in web applications. If you've ever worked with a large-scale application, you can easily see how it happens.

By following OWASP's recommendations above, **you can help reduce the likelihood your application will contain broken access control, but you can never fully prevent it**.

As always, do adequate research and craft a security plan that works for your application and your organization.

#### Further Reading
- [OWASP's Access Control Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html){:target="_blank"}