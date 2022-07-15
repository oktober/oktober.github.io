---
layout: post
title: OWASP Top Ten - Software and Data Integrity Failures
tags: tech
description: 
thumbnail: 
thumbnail-creator-link: 
thumbnail-creator: 
thumbnail-site: 
thumbnail-site-name: 
image-alt: 
date: 2022-07-07

---

This new addition to the 2021 OWASP Top Ten list - Software and Data Integrity Failures - encompasses integrity violations for code and infrastructure.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## What are Software and Data Integrity Failures?
This security risk encompasses all the various integrity failures that can occur in the software development lifecycle. 

Some common integrity failures are:
- [Insecure Deserialization]({{ site.baseurl }}{% link _owasp-top-10/Insecure-Deserialization.md %}) - this was a standalone risk in the 2017 Top Ten, but has been rolled into this more generic category for 2021
- Including 3rd party content in your application from sources you don't control
- Not properly securing your CI/CD pipeline
- Automatically updating software without performing sufficient integrity checks

---

## Possible Ways to Help Prevent Software and Data Integrity Failures
[OWASP recommends](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/#how-to-prevent){:target="_blank"} at least 6 actions you can take to help prevent this security risk:
- Using digitial signatures (or something similar) to verify the software or data has not been altered
- Ensuring libraries and dependencies are using trusted repositories
- Using a software supply chain security tool to verify that components do not contain known vulnerabilities
- Implementing a review process for code and configuration changes to reduce the chance that malicious code or configurations can be introduced
- Ensuring your CI/CD pipeline has proper segregation, configuration, and access control
- Ensuring that unsigned or unencrypted serialized data always has some form of integrity check or digital signature to detect tampering or replay of the serialized data


---

## Sum It Up
Pretty much every piece of technology is built with 3rd party components. We all rely on updates to keep our software secure. Quite often we need to serialize data for transport. But there are risks to these activities.

By being aware of these software and data integrity risks, we can put process in place to help reduce our risk. 

Every app or company is a potential target and this is an ever-present and growing threat.

Check out the resources below to help further your knowledge and keep learning so you can build customized solutions that work for your situation.

#### Further Reading
- [OWASP Top Ten #8 Software and Data Integrity Failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/){:target="_blank"}
- [OWASP Cheat Sheet: Deserialization](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html){:target="_blank"}
- [OWASP Cheat Sheet: Infrastructure as Code](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html){:target="_blank"}
- [SAFECode Software Integrity Controls (PDF)](https://safecode.org/publication/SAFECode_Software_Integrity_Controls0610.pdf){:target="_blank"}