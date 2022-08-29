---
layout: post
title: Cross-Origin Resource Sharing (CORS) Configuration Vulnerabilities - Part 1
tags: tech cybersecurity
description: 
thumbnail: /post-thumbnails/2022-08-15-cors-configuration-vulnerabilities-part-1-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@vonshnauzer
thumbnail-creator: Egor Myznik
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Precarious bridge hanging high up in the sky
  
---

Cross-Origin Resource Sharing (CORS) is a controlled relaxation of the security  Same Origin Policy (SOP) provides. When done properly, it can enable cross-origin functionality across the web.

If not done properly, then security vulnerabilities can easily arise and attackers are more than willing to take advantage.

<!--more-->

{% include toc.md %}

---

## Prerequisites

To help understand the concepts in this article, you should already be familiar with:
- [How the web works: HTTP requests & responses]({{ site.baseurl }}/How-The-Web-Works){:target="_blank"}
- [Basic understanding of Javascript {Mozilla}](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide){:target="_blank"}
- [Same Origin Policy]({{ site.baseurl }}/Same-Origin-Policy){:target="_blank"}
- [Cross-Origin Resource Sharing headers]({{ site.baseurl }}/CORS-Headers){:target="_blank"}

---

## Risks of a CORS vulnerability
CORS relaxes the security of the SOP. This means if something does go wrong in the implementation of CORS, we don't have the SOP to protect us.

To understand the types of attacks that are used with CORS vulnerabilities, you need to know what a [Cross-Site Request Forgery attack (CSRF)](https://portswigger.net/web-security/csrf){:target="_blank"} is.

On a high level, a CSRF attack occurs when: 
- a user is logged into a legitimate website (Website A)
- user then visits a malicious website (Website B) 
- malicious website B has Javascript that automatically runs to submit a GET or POST request to website A, acting as the user

If Website A is not designed securely, the Javascript on Website B could perform actions as the user, such as submitting an HTTP request to change the user's password and take over their account.

**This is all valid behavior _even with SOP_**. But, SOP doesn't allow the attacker to read the HTTP response. It's a blind attack. They can submit the GET or POST request, but they can't read the response.

When CORS is implemented improperly, sensitive data can be read and CSRF attacks can still happen plus **the attacker can now read the HTTP response**. 

The attack is now much more damaging and **even allows attackers to get around traditional CSRF mitigation techniques like anti-CSRF tokens**. Anti-CSRF tokens are no good if an attacker can just request the page first and read the token.

**CORS vulnerabilities remove _all_ protections provided by the SOP** and can give an attacker full access to accounts.

This is why it's so important that if we use it, we need to try to implement it as securely as possibly.

---

## Most common CORS vulnerabilities
Now we know what's at stake with a CORS vulnerability, how do they happen?

Well, most risks occur because you, as a web developer, want to set `Access-Control-Allow-Credentials: true` but you also need to allow _multiple_ origins. Since the specification only allows you to set one origin, you have to take the `Origin` value from the HTTP request and use that to set the `Access-Control-Allow-Origin` header.

If you need to dynamically generate the origin value (and required credentials), be sure that you:
- Don't just reflect back whatever `Origin` you received
- Always use an approved list to check the `Origin` against
- Thoroughly test that your validation for the approved list works properly
- Set the `Vary: Origin` header to prevent cache poisoning attacks and other caching issues

In the following sections we'll discuss what can happen if we don't do these things properly.

---

**_Note:_ All the risks in this article occur because we're trying to restrict access to only authorized users and so we've set `Access-Control-Allow-Credentials: true`**

---

## Just reflecting back the origin value
Since you can't use the wildcard value for the `Access-Control-Allow-Origin` header, you figure why not create your own wildcard functionality and just reflect back the `Origin` value from the request?

If you do this, and you require `Access-Control-Allow-Credentials: true`, you're essentially giving permission to every website to submit a request, on behalf of your user, using the user's credentials.

Before you do this, stop and think - is that really what you want?

Chances are, you don't. Every website means even malicious websites. Just like in the example above, those malicious websites would love their domain/origin to be allowed to submit actions for any user visiting their page who is also logged into your app.

Keeping that attack vector in mind, **this is probably not the functionality you want to implement**.

### One possible use case
The only way this might be okay is if _none_ of your credentials are used for authorization. Maybe they're all used for tracking purposes instead. 

But this would require you to know, _without a doubt_, that any possible authentication credential for this resource has the `SameSite` attribute set to `Lax` or `Strict` to ensure it won't be sent. This is a risky assumption though and for most cases, I wouldn't recommend it.

### Alternate recommendations
Instead, it's much safer to **create a list of approved origins** that are allowed to access the resource and **check the origin against that**. 

Or, if you don't need credentials, don't require them. Set `Access-Control-Allow-Origin: *` and then everyone knows to treat this as a publicly accessible resource.

---

## Improperly validating the origin value
If you took my advice above and decided to make an approved list of origins that are allowed to access this resource, then you'll need to pay close attention to this section and really understand what can go wrong.

Validation is not always easy. Attackers know this and are constantly on the lookout for validation that doesn't quite work how the developer thought it would work.

If you validate the origin against an approved list, you need to make sure you're **validating the _entire origin_**.

If you only check that the first or the last part matches the origin, an attacker can create a domain to meet those requirements.

### Only matching the first part of the origin
Let's say you misunderstood the validation happening and you're really only checking that the first part of the origin matches one in your approved list. How can an attacker abuse this?

Let's say an allowed origin in your list is `https://twitter.com`. You didn't realize it, but your validation is really only checking that the origin starts with `https://twitter.com`.

An attacker figures this out and registers a subdomain on their own URL called `twitter.com.malicioussite.com`. Now when they perform a CSRF attack on one of your logged-in users, their malicious origin will validate properly. They now have access to read cross-origin resources accessible to that user and perform actions as them.

### Only matching the last part of the origin
Let's say you want to allow any subdomains from `example.com` to access this resource using credentials. To set up a wildcard type of situation, you validate that the URL has to end with `example.com`.

In this scenario, you're not accurately checking only for subdomains to `example.com`. You're just checking that a domain ends with that URL. 

An attacker could register the domain `foobarexample.com` and that will pass your validation.

#### What about subdomain takeovers?

Let's say you did set up the subdomain validation properly and it properly checks that the URL ends with `.example.com`.


Have you thought about the potential risks of [subdomain takeovers](https://0xpatrik.com/subdomain-takeover-basics/){:target="_blank"}? Can you ensure that _none_ of your subdomains are vulnerable? 

Granted, if an attacker has access to one of your subdomains, you've got even bigger problems. But it's a potential attack vector you should be aware of when implementing your CORS validation as it could put your users at considerable risk.

### How to properly validate the origin value
Essentially, you want your validation to check **the entire origin** and make sure **it matches a value in your approved list**. 

If you have to account for variable data such as any subdomains or port numbers, make sure to use an allowed list of characters, not a block list. Read through [this article by Corben Leo](https://corben.io/advanced-cors-techniques/){:target="_blank"} to see how using a blocklist for variable characters can lead to an exploit.


**Test your validation thoroughly**. Make sure it works how you expect it to.

All these guidelines around validation are definitely easier said then done. Sometimes you don't know exactly what exact origin the request will have or it could change in the future. Adapting to the different scenarios for your exact situation is difficult.

But the vulnerabilities that can arise from misconfigured CORS can be so damaging to your users, that it's important to understand the risks that are out there and to try and mitigate them as much as you can.

---

## Not setting the `Vary: Origin` header
If you must use the `Origin` value from the HTTP request to dynamically generate the `Access-Control-Allow-Origin:` value, you need to add `Vary: Origin` to your response.

The `Vary` response header tells caches to include any headers listed as part of their cache key. 

A cache key is made up of the HTTP method, resource path, the `Host` header value, plus any headers listed in `Vary`. The caching resource combines these values and will cache them for any other request containing the exact same values.

For example, let's say a user navigates to `https://example.com/` and their browser creates the following abbreviated request:
```
GET / HTTP/2
Host: example.com
User-Agent: Mozilla/5.0
...
```
The web server then generates the following abbreviated response:
```
HTTP/2 200 OK
Content-type: text/html; charset=utf-8
...
```
The cache provider will look at the HTTP request method _(GET)_, resource path _(/)_, and `Host` value _(example.com)_ to create the cache key. 

Any other user whose request matches the cache key (i.e. they do a `GET` request to the root directory of `example.com`) will receive a cached copy of the response.

But if we add a `Vary` header to the response, like so:
```
HTTP/2 200 OK
Content-type: text/html; charset=utf-8
Vary: User-Agent
...
```
Now the cache provider will use the method, resource path, `Host` value, AND the `User-Agent` value to create the cache key. Any other user who does a `GET` request to the root directory of `example.com` and is using 'Mozilla/5.0' as their `User-Agent` value will receive a cached copy of the response.

But, any user using a different `User-Agent` will NOT receive this cached copy of the page. Their request will have to go directly to the web server to get an HTTP response. All because we told the cache provider to include the `User-Agent` value as part of the cache key

So, if we're using the `Origin` value from the request to dynamically generate the `Access-Control-Allow-Origin:` value, we need to add `Origin` to the `Vary` header, like so:
```
HTTP/2 200 OK
Content-type: text/html; charset=utf-8
Access-Control-Allow-Origin: https://staciefarmer.com
Vary: Origin
...
```
This way requests from one origin don't get accidentally cached and retrieved for requests from ALL origins. 

This in itself is not a security vulnerability, but not setting the `Vary: Origin` header when using the request's `Origin` value to dynamically generate the `Access-Control-Allow-Origin:` value can lead to other security vulnerabilities.

### Cache Poisoning
Cache poisoning happens when unkeyed input contains data that is used somewhere in your application. 

What's unkeyed input? It's any header that affects the response, but hasn't been added to the `Vary` header so it's used as part of the cache key.

Cache poisoning basically saves an attacker's payload in the cache. Then, when other unsuspecting users visit the site, they're served the cached page and the attack occurs. This could be a stored XSS attack, which is probably most common, but it can be a business logic attack depending on how you're application uses the data.

If you're dynamically generating the `Access-Control-Allow-Origin:` using the request's `Origin` value, that means you're taking unkeyed input _(e.g. the value in the `Origin` header)_ and using it within your application. If you don't add `Origin` to the `Vary` header, then you put all your users at risk of a possible cache poisoning attack.

Check out [this article by James Kettle](https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties){:target="_blank"} to learn more about CORS and the client-side and server-side cache poisoning attacks that are possible.

---

## Continue Reading
Keep reading about [more CORS Vulnerabilities in Part 2]({{ site.baseurl }}/CORS-Configuration-Vulnerabilities-Part-2).