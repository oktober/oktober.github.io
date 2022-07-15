---
layout: post
title: OWASP Top Ten - Server-Side Request Forgery (SSRF)
tags: tech
description: 
thumbnail: 
thumbnail-creator-link: 
thumbnail-creator: 
thumbnail-site: 
thumbnail-site-name: 
image-alt: 
date: 2022-07-11

---

Server-Side Request Forgery is a difficult vulnerability to find and challenging to exploit. But when an attacker does find one, it could allow them to bypass access controls, leak confidential data, and even execute code - all using your server. It's a welcome addition to the 2021 OWASP Top Ten list.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## What is Server-Side Request Forgery?
SSRF (Server-Side Request Forgery), commonly happens because a URL, called by the app, can be modified by the client.

For example, imagine your web app needs to get stock info from an external API - like the example from [PortSwigger here](https://portswigger.net/web-security/ssrf){:target="_blank"}.

When a customer clicks on a button to 'Check Availability', that triggers a POST request to the external API - http://stock.weliketoshop.net:8080/product/stock/check?productId=6&storeId=1

Instead of hard-coding the API's URL, it's referenced in the HTTP Request, like this:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```

Since it's in the HTTP Request, this URL can be modified by a user (or an attacker). Since your web server is the one making the call to the URL, this can allow an attacker to use your server to call external or internal URL's.

For example, what if the attacker intercepted that POST Request and changed it to localhost, like this:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost
```

But `localhost` is protected against external access, you might say.

You forget - this is your web server making the call to localhost. Your web server is on your internal network. An attacker is simply using your web server to make its request for them.

### What's the harm with accessing `localhost`?
Depending on how your network is set up, you could have a lot of goodies on `localhost` that you don't want external entities accessing. 

Maybe you have:
- localhost/phpmyadmin
	- you wouldn't want an attacker gaining access to that page then brute forcing usernames and passwords
- localhost/admin
	- you may not even set up access control to certain internal pages, because you think there's no way someone can get through your firewall

The list can go on and on. An attacker could even enumerate through the port numbers to see what services you do have available internally and see if any of them have exploitable vulnerabilities.

There's a good chance you might have AWS metadata stored on `169.254.169.254`. An attacker who's found an SSRF vulnerability is highly likely to check there.

Once an attacker gets in, there is a lot they could potentially do.

---

## More Advanced SSRF Attacks
Finding an exploitable URL field is a very common way to exploit SSRF. But there are also more advanced ways to do it that you don't think about.

How about files that are parsed by the application? Maybe there's a PDF generator or your app allows customers to upload PDFs, Word Docs, or Excel files. Each of those types of files are a collection of XML data behind the scenes and could contain XXE (XML External Entity) vulnerabilities that allow SSRF.

For example, NahamSec's post - [My Expense Report Resulted in a Server-Side Request Forgery (SSRF) on Lyft](https://nahamsec.com/posts/my-expense-report-resulted-in-a-server-side-request-forgery-ssrf-on-lyft){:target="_blank"}, clearly explains how he found a way to inject `<link>` tags into a PDF generator (WeasyPrint) on Lyft's app. This was not a simple attack and it definitely took a lot of time and investigating to figure out all the pieces that needed to be put in place, but ultimately it gave him access to the internal network and he was able to view the AWS metadata instance keys.

---

## Possible Ways to Help Prevent Server-Side Request Forgery
As usual, this is a tough one to prevent completely, but the following tips [from OWASP](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/#how-to-prevent){:target="_blank"} can help:
- Network Side:
	- Segment remote resource access functionality in separate networks
	- Enforce “deny by default” firewall policies or network access control rules to block all but essential intranet traffic
- Application Side:
	- Sanitize and validate all client-supplied input data
	- Enforce the URL schema, port, and destination with a positive allow list
		- Do not mitigate SSRF via the use of a deny list or regular expression. Attackers have payload lists, tools, and skills to bypass deny lists.
    - Do not send raw responses to clients
	- Disable HTTP redirections
    - Be aware of the URL consistency to avoid attacks such as DNS rebinding and “time of check, time of use” (TOCTOU) race conditions

Another strong protection option could be to use a helper server to fetch any external resources the web app needs. The helper server would be locked down to have no access to surrounding internal services and its sole function would be to retrieve resources for the web app server. This doesn't prevent all SSRF attacks, but could greatly reduce the attack surface for certain companies. More information about this method can be found in this white paper - [Preventing Server-Side Request Forgery Attacks](http://seclab.nu/static/publications/sac21-prevent-ssrf.pdf){:target="_blank"}

---

## Sum It Up
SSRF vulnerabilities can be a nasty beast if attackers find them in your web app. The likelihood can be low, but the impact can be high, so it's important to understand how these types of vulnerabilities happen and what you can do to prevent them.

Check out the resources below to help further your knowledge and keep learning so you can build customized solutions that work for your situation.

#### Further Reading
- [OWASP Top Ten #9 Server-Side Request Forgery (SSRF)](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_(SSRF)/){:target="_blank"}
- [PortSwigger - Server-Side Request Forgery](https://portswigger.net/web-security/ssrf){:target="_blank"}
- [OWASP Cheatsheet - SSRF Bible](https://cheatsheetseries.owasp.org/assets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet_SSRF_Bible.pdf){:target="_blank"}
- [@jdonsec's List of SSRF Writeups and Resources](https://github.com/jdonsec/AllThingsSSRF){:target="_blank"}