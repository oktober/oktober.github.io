---
layout: post
title: Cross-Origin Resource Sharing (CORS) Configuration Vulnerabilities - Part 2
tags: tech cybersecurity
description: 
thumbnail: /post-thumbnails/2022-08-16-cors-configuration-vulnerabilities-part-2-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@yannallegre
thumbnail-creator: Yann Allegre
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Precarious train track winding through snowy mountain
  
---

In our last article, [Cross-Origin Resource Sharing (CORS) Vulnerabilities - Part 1]({{ site.baseurl }}/CORS-Configuration-Vulnerabilities-Part-1), we discussed the vulnerabilities that can happen when you dynamically generate the `Access-Control-Allow-Origin` value using the request's `Origin` value.

In this article we'll discuss the other situations you should be aware of where using CORS can increase your application's security risks.

<!--more-->

{% include toc.md %}

---

## Prerequisites

To help understand the concepts in this article, you should already be familiar with:
- [How the web works: HTTP requests & responses]({{ site.baseurl }}/How-The-Web-Works){:target="_blank"}
- [Basic understanding of Javascript {Mozilla}](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide){:target="_blank"}
- [Same Origin Policy]({{ site.baseurl }}/Same-Origin-Policy){:target="_blank"}
- [Cross-Origin Resource Sharing headers]({{ site.baseurl }}/CORS-Headers){:target="_blank"}

We also recommend reading the article for [CORS Configuration Vulnerabilities - Part 1]({{ site.baseurl }}/CORS-Configuration-Vulnerabilities-Part-1){:target="_blank"} first, if you haven't already.

---

## Using `null` with credentials
Some apps add `null` to their origin allow/approved list while also setting `Access-Control-Allow-Credentials: true`.

If you're a developer working on one of those apps, you need to be aware of the potential attack vector you've just opened up.

First, let's talk about when the `Origin` value might be set to `null` by the browser.

### When do browsers set `Origin: null`?
[Portswigger says](https://portswigger.net/web-security/cors#whitelisted-null-origin-value){:target="_blank"} the browser might send `Origin: null` when it's a:
- cross-origin redirect
- request from serialized data
- request using the file: protocol
- sandboxed cross-origin request

This last option - **a sandboxed cross-origin request** - is the easiest way for attackers to exploit an application that has added `null` to their approved list and requests credentials.

### Sandbox attack

If an attacker can get a logged in user of your app to visit their malicious site, they can trigger a cross-origin request using Javascript from a sandboxed iframe. This can cause the `Origin` to be set to `null` by the browser.

To see the code that can trigger this attack, visit [Portswigger's article on CORS](https://portswigger.net/web-security/cors#whitelisted-null-origin-value){:target="_blank"}.

If you have `null` as a trusted value for `Origin` and are requiring credentials to access the resource, you need to be aware of this easily exploitable vulnerability.

---

## Risks with trusting HTTP origins
If you add HTTP origins to your approved list, your users are open to the same types of attacks we mentioned above. Only this time, they don't have to visit a malicious website. They can be performed by a [Monster-in-the-Middle (MiTM)](https://en.wikipedia.org/wiki/Man-in-the-middle_attack){:target="_blank"}.

Visit [PortSwigger's article](https://portswigger.net/web-security/cors#breaking-tls-with-poorly-configured-cors){:target="_blank"} to read more about how this attack can occur.

As a general best practice, you should only trust HTTPS sites to help keep your users safe.

---

## Intranets and CORS vulnerabilities
With attacks utilizing CORS vulnerabilities, you have to remember - **the attacker is acting within the _user's context_**. Meaning, if a user can access a cross-origin resource and if they visit a malicious website, then **the attacker can also access that cross-origin resource** as if they were the user.

This is important to remember on the internet and also any intranets that user may be on.

If there is an application that allows cross-origin requests on the local network _(i.e. not the public internet)_ and the user is on that network and has authorization to access resources on that application, it could be vulnerable to these same types of attacks.

For example, [a vulnerability with the JetBrains IDE was discovered in 2016 by Jordan Milne](http://blog.saynotolinux.com/blog/2016/08/15/jetbrains-ide-remote-code-execution-and-local-file-disclosure-vulnerability-analysis/){:target="_blank"}. This vulnerability occurred because the Integrated Development Environment (IDE) spun up a local web server for devs to preview their apps. The resources on the local web server also added the header `Access-Control-Allow-Origin: *`.

If a dev who had that web server running on their IDE were to visit a malicious site on the internet - and the attacker knew how to test for and run this exploit - that attacker could access the dev's code and also the files for their operating system. You can imagine how problematic that would be.

In this case, Milne was able to show how an attack could lead to remote code execution. Not all situations will be quite that bad, but it's important to understand that **adding CORS headers can remove other network security precautions** already in place.

---

## Risks with trusting any origin
One last note to touch on - even if you implement CORS properly, **any origin you trust could be an attack vector**. 

For example, let's say you trust `vulnerable-example.com` but you didn't know it has some pretty serious [Cross-Site Scripting (XSS)]({{ site.baseurl }}/owasp-top-10/Cross-Site-Scripting/) vulnerabilities.

An attacker knows this and has stored some malicious Javascript to perform CSRF attacks in `vulnerable-example.com`.

One of your users is logged into your site. Then they open a new tab and visit `vulnerable-example.com`. The stored XSS is triggered and the attacker retrieves the anti-CSRF tokens for your 'Change Password' page. The attacker's Javascript then uses those same tokens to change the user's password and takes over their account.

Granted, this is just an example and attacks are not always simple and straight forward. But attackers are clever and persistent, especially when they're motivated. 

Any origins you trust in your approved list for authorized resources should be considered a potential attack vector.

---

## Wrap It Up
CORS is a useful tool for the interconnectedness of the web. But it requires a weakening of a policy that helps keep the web safe - Same-Origin Policy.

This means if you use CORS, ensure you:
- Understand any risks you might be taking
- Mitigate those risks as much as you can
- Know which ones you've decided to accept
	- Understand the consequences this may have for your users and your business

Check out the resources below to help further your knowledge and keep learning so you can better protect your users.

### Further Reading
- [What is CORS (cross-origin resource sharing)? {PortSwigger}](https://portswigger.net/web-security/cors){:target="_blank"} 
- [Exploiting CORS misconfigurations for Bitcoins and bounties by James Kettle {PortSwigger}](https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties){:target="_blank"}  
	- Conference talk: [Exploiting CORS misconfigurations for Bitcoins and bounties by James Kettle {YouTube}](https://www.youtube.com/watch?v=wgkj4ZgxI4c){:target="_blank"}
- [Advanced CORS exploitation techniques by Corben Leo](https://corben.io/advanced-cors-techniques/){:target="_blank"}