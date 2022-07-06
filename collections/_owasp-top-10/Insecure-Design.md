---
layout: post
title: OWASP Top Ten - Insecure Design
tags: tech
description: 
thumbnail: 
thumbnail-creator-link: 
thumbnail-creator: 
thumbnail-site: 
thumbnail-site-name: 
image-alt: 
date: 2022-07-06

---

Insecure design happens before we even start coding - during the design phase of an application. Before even one pieces of code is written, do you know what risks could arise and how you should mitigate them? If not, then you're one of many who have fallen victim to insecure design and have put your application at risk.

A key step that should occur during the design phase of an application is business risk profiling - what type of software is being developed and what level of security design does it require?

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## What is Insecure Design?
Insecure design is all about identifying what risks exist during the design phase so mitigating factors are built in from the start.

### Questions to Ask
According to [OWASP](https://owasp.org/Top10/A04_2021-Insecure_Design/){:target="_blank"}, during the design phase you should be asking these questions to determine what types of mitigations you need and how you might go about implementing them:

- What type of software is being developed and what level of security design is required?
- Do you know the business requirements including protections for confidentiality, integrity, availability, and authenticity of all data assets and the expected business logic?
- Have you considered how exposed your application will be and if you need segregation of tenants?
- How will you continually evaluate threats and ensure your code is designed and tested to prevent know attacks?

Security shouldn't just be baked into the coding process. It should be integrated into all parts of the software development lifecycle to help keep your applications safe.

---

## Possible Ways to Help Prevent Insecure Design
Involving security professionals, and integrating security, in all parts of the software development lifecycle will help you avoid the vulnerabilities that can arise from insecure design.

Some other ways to help prevent this vulnerability, [according to OWASP](https://owasp.org/Top10/A04_2021-Insecure_Design/){:target="_blank"}, are:
- Develop a library of, and use, secure design patterns and code
- Use threat modeling for critical parts of your application like authentication, access control, business logic, and key flows
- Integrate security language and controls into user stories
- Add security checks to your unit and integration tests

Find more ways to prevent insecure design in [OWASP's article](https://owasp.org/Top10/A04_2021-Insecure_Design/){:target="_blank"}.


---

## Sum It Up
Security should be involved in all levels of an application - including the design phase. 

By overlooking this point in the process, you could set your company up for security vulnerabilities down the road. We know better, now we can do better.

Check out the resources below to help further your knowledge and keep learning so you can build customized solutions that work for your situation.

#### Further Reading
- [OWASP Top Ten #4 Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/){:target="_blank"}
- [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org/){:target="_blank"}
- [Resources to Learn About Threat Modeling](https://github.com/hysnsec/awesome-threat-modelling){:target="_blank"}