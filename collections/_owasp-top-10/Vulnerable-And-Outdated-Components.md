---
layout: post
title: OWASP Top Ten - Vulnerable and Outdated Components
tags: tech
description: 
thumbnail: /9-using-components-with-known-vulnerabilities/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@markusspiske
thumbnail-creator: Markus Spiske
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Symbol of person holding up a hand as a warning to stop
date: 2022-07-05
  
---

Components are used in practically every codebase. Unfortunately, this also means known vulnerabilities exist in many of those components and/or they're out of date. These vulnerable and outdated components put the entire application/API at risk.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## Using Components with Known Vulnerabilities
We all probably use other people's code _(components)_ in our application or API, but **most of us probably don't know:**
- all the different components, and their dependencies, in use
- if they need to be updated
- if they're no longer being maintained

If you know all 3 of those above you're in excellent shape and probably don't need to keep reading.

If you're unsure about any of those areas, then you should definitely keep reading.

### What are Components?
Essentially, components are blocks of 3rd party code that you integrate into your codebase.

In WordPress, components would be like plugins. They're built by 3rd parties and you can plug them into your codebase to expand its functionality.

In many applications, components are 3rd party libraries or a larger self-contained package of software you can include in your codebase. jQuery is a good example of this type of component. It's a popular library that makes writing Javascript easier and is commonly used in many web applications.

### Benefits vs Risks of Components
Components can offer lots of benefits. They allow us to quickly expand the functionality of our code and can even increase its security. 

We do not, and should not, write our own encryption libraries. Instead, we use reputable 3rd party libraries whose focus is solely on encryption and, hopefully, know how to do it right.

But, like everything, using components introduces risks.

Because this is code written by someone else, we probably don't know if it was written securely or if it currently contains vulnerabilities. We may not know if the project was recently taken over by a malicious actor or if new vulnerabilities were accidentally introduced in the latest update.

Components are often open source and used in many applications across the web. This makes it easier for attackers to find and exploit vulnerabilities.

---

## Testing for Known Vulnerabilities in Components
While prevention is much more effective than testing, you should still **perform regular vulnerability scanning as an additional layer of defense**.

---

## Possible Ways to Help Prevent Using Components with Known Vulnerabilities
[According to OWASP](https://owasp.org/www-project-top-ten/2017/A9_2017-Using_Components_with_Known_Vulnerabilities){:target="_blank"}, there are a few things you should be doing to identify and prevent component vulnerabilities from putting your application or API at risk.

1. **Continuously identify all components in use and their dependencies**  
Just like with asset management, we need to know what we're using - especially when things change. Some tools that can help with this include [versions](https://www.mojohaus.org/versions-maven-plugin/){:target="_blank"}, [DependencyCheck](https://owasp.org/www-project-dependency-check){:target="_blank"}, and [retire.js](https://github.com/retirejs/retire.js/){:target="_blank"}.

2. **Monitor for known vulnerabilities**  
Specifically, you should subscribe to security bulletins for components in use and their dependencies. You can also regularly check [CVE](https://cve.mitre.org/){:target="_blank"} and [NVD](https://nvd.nist.gov/){:target="_blank"} for known vulnerabilities.

3. **Monitor for unmaintained or unpatched components**  
Would you know if a component in use or its dependency stops being maintained? Would you know if a component or its dependency stopped releasing security patches? This is a little tougher to keep track of, but it's important to know if a component you use, and its dependencies, are still being regularly maintained and patched.

4. **Apply software patches quickly**  
Once you know you have a vulnerable component, time is of the essence. Try to patch it as quickly as you can. If, for whatever reason, patches can't be applied, consider using [virtual patches](https://owasp.org/www-community/Virtual_Patching_Best_Practices){:target="_blank"}.

5. **Periodically remove unused components**  
Good patch management includes keeping things cleaned up. If you have a good process for identifying and removing unused components, that will greatly improve your security and make ongoing patch management much easier.

---

## Sum It Up
We all use components in our codebases. They make things easier, but they also introduce risks.

Keeping an eye on things with a well functioning patch management and monitoring system will help to greatly reduce the risk of using components with known vulnerabilities.

Like everything with security, this topic can be complex. Check out the resources below to help further your knowledge and keep learning so you can build customized solutions that work for your situation.

#### Further Reading
- [OWASP - #6 Using Components with Known Vulnerabilities](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/){:target="_blank"}