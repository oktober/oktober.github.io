---
layout: post
title: OWASP Top Ten - Security Misconfiguration
tags: tech
description: 
thumbnail: /6-security-misconfiguration/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@amoltyagi2
thumbnail-creator: Amol Tyagi
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Key left in lock
date: 2021-01-14
---

Security misconfiguration is a pretty broad category. It's kind of a catch-all. It includes some of the other OWASP Top Ten vulnerabilities such as sensitive data exposure, broken authorization, etc. It also includes a few more scenarios that don't quite fit in any other category such as error handling, unnecessary open ports, etc. 

In 2021, OWASP also folded <a href="{{ site.baseurl }}/owasp-top-10/XML-External-Entities">XML External Entities (XXE)</a> into Security Misconfiguration as part of their Top Ten list.

<!--more-->

{% include toc.md %}

---

## Common Security Misconfigurations
[According to OWASP](https://owasp.org/www-project-top-ten/2017/A6_2017-Security_Misconfiguration){:target="_blank"}, some common examples of security misconfigurations you might find are:
- Missing security hardening or improperly configured permissions on cloud services
- Unnecessary features enabled/installed *(e.g. ports, services, pages, accounts, or privileges)*
- Default/Hardcoded accounts present
- Error handling revealing too much information
- Insecure values being used in application servers, frameworks, libraries, databases, etc
- Security headers or directives are not being used or are being implemented improperly
- Using software with known vulnerabilities that aren't patched (or the patches haven't been implemented)

When testing applications for vulnerabilities, one of the first things we'll do is check for open ports, default accounts, and out-of-date software. **These things can be relatively easy to find** and can create small or very large risks (e.g. being used to gain admin access to a system).

---

## Testing for Security Misconfigurations
Chances are, your application has some form of security misconfiguration going on.

**OWASP has some great resources to help you test your own application** and hopefully find the vulnerabilities before others do.
- [Configuration and Deployment Management Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/README){:target="_blank"}
- [Error Handling Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/){:target="_blank"}
- [Configuration Verification Requirement (PDF)](https://raw.githubusercontent.com/OWASP/ASVS/v4.0.2/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0.2-en.pdf){:target="_blank"}

---

## Ways to Help Prevent Security Misconfiguration
Since this category is so broad, it can be very difficult to prevent. Still, we need to try.

[OWASP has some great tips](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/#how-to-prevent){:target="_blank"} on ways you can prevent these types of misconfigurations:
- Create an automated hardening process for creating and deploying new, secure environments.
- Ensure your platform/application/etc is set up with only the absolute necessary features. Remove any unnecessary features/frameworks/etc.
- Regularly review and update configurations for security notes, updates, and patches - especially cloud storage permissions.
- If you can, segment your application architecture.
- Use security directives such as [Security Headers](https://owasp.org/www-project-secure-headers){:target="_blank"}.
- Implement automated testing to check the security configurations and settings of all your environments.

These are just a few of the things that can help. **Check out the resources below** to help expand your knowledge and create a customized process that works for your situation.

#### Further Reading
- [OWASP - #5 Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/){:target="_blank"}
- [NIST Guide to General Server Hardening](https://csrc.nist.gov/publications/detail/sp/800-123/final){:target="_blank"}
- [CIS Security Configuration Guides/Benchmark](https://www.cisecurity.org/cis-benchmarks/){:target="_blank"}
- [OWASP Configuration Verification Requirement (PDF - *starting on p.59*)](https://raw.githubusercontent.com/OWASP/ASVS/v4.0.2/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0.2-en.pdf){:target="_blank"}