---
layout: post
title: Cross-Origin Resource Sharing (CORS) Headers
tags: tech cybersecurity
description: 
thumbnail: /post-thumbnails/2022-08-14-cors-headers-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@yanivcphoto
thumbnail-creator: Yaniv Cohen
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Car crossing a bridge
  
---

Same Origin Policy (SOP) is a security feature that prevents other websites from reading resources on your site. But what if you want them to read some resources on your site? What if you have a public API you want other websites to interact with? Or what if you made a cool web font for other websites to use?

You can loosen the restrictions of SOP and let other sites access your resources with Cross-Origin Resource Sharing (CORS) headers.

<!--more-->

{% include toc.md %}

---

## Who Is This Article For?
There are 2 different use cases for CORS. You're either **the application opening up access to your resources** or you're **the application _accessing_ someone else's resources**.

In this article, we'll be focusing on the former and assume you're trying to securely allow access to your application resources.

So let's talk about the CORS response headers and how you can use them.

---

## CORS Response Headers
There are 6 response headers you can use to allow cross-origin access to your resource(s). One is required, the rest are optional:
- **Access-Control-Allow-Origin** **_{required}_**
	- Which origins are allowed to access this resource?
- **Access-Control-Expose-Headers**
	- Which response headers can Javascript read?
- **Access-Control-Max-Age**
	- How long with the results of a preflight request be cached?
- **Access-Control-Allow-Credentials**
	- Are valid credentials required to access this resource?
- **Access-Control-Allow-Methods**
	- What HTTP methods are allowed?
- **Access-Control-Allow-Headers**
	- Which HTTP headers are allowed?

Since the security vulnerabilities I've seen so far only involve misconfiguration of `Access-Control-Allow-Origin` and `Access-Control-Allow-Credentials`, we'll only be discussing those 2 headers in this article.

You should still research and understand the other headers to see if they are needed for your situation.

### Further Reading
- [Mozilla's Guide to CORS - Response Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers){:target="_blank"}
- [WhatWG's Fetch Spec - CORS Response Headers](https://fetch.spec.whatwg.org/#http-responses){:target="_blank"}

---

## Header: `Access-Control-Allow-Origin`
This is the only header you must set to allow cross-origin access to resource(s) on your server.

You have 3 options for this header.

### 1. `Access-Control-Allow-Origin: *`
You can set the origin to be a wildcard - meaning _any_ origin can access this resource. 

This is ideal for public resources that **don't require credentials**.

If you also set `Access-Control-Allow-Credentials: true`, then you cannot use a wildcard for this header. You must use one of the options below.

### 2. `Access-Control-Allow-Origin: <origin>`
You can set the value to be a single origin.

Unfortunately, most of us need to specify a list of origins, not just one. 

However, if you also require credentials to access this resource, then you'll probably by dynamically generating the value for this header. And that's where security vulnerabilities start to creep in.

If you need to allow (credentialed) cross-origin access to multiple origins:
- Review the [basic recommendations listed below]({{ site.baseurl }}/CORS-Headers/#recommendations-when-dynamically-generating-the-origin-value)
- Familiarize yourself with [the vulnerabilities that can happen]({{ site.baseurl }}/CORS-Configuration-Vulnerabilities-Part-1/) when dynamically setting this value
- Test thoroughly

### 3. `Access-Control-Allow-Origin: null`
This is an option, but if you also set `Access-Control-Allow-Credentials: true`, **it's not recommended**. 

Your application is vulnerable to even more damaging [Cross-Site Request Forgery (CSRF)](https://portswigger.net/web-security/csrf){:target="_blank"} type of attacks if you: 
- set `Access-Control-Allow-Credentials: true`
- set `Access-Control-Allow-Origin: null`
- and your application can be displayed in an iframe

The CSRF-type attack is more damaging because the attacker now also has full access to read the HTTP responses. This could essentially give the attacker full control over a logged in user's account.

In general, you don't want to set the allowed origin to `null` (if you require credentials for a resource) unless you've fully accepted the security risks or mitigated them some other way.

And if you don't require credentials for that resource, then just set it to a wildcard (*).

---

## Header: `Access-Control-Allow-Credentials: true`
If you set this header, you're telling the browser to attach the user's credentials _(i.e. cookies or other authentication info)_ whenever a cross-origin HTTP request is sent to your app.

This attribute can introduce some very damaging security vulnerabilities for your users, so **be careful when and how you use it**.

A couple of things to consider when you set this header:
- **The `Access-Control-Allow-Origin` value must be _a single origin or `null`_**
	- If you set `Access-Control-Allow-Origin: *` and require credentials, the browser will not let Javascript read the HTTP response and it will report a CORS error in the console
- **SameSite attribute on the cookie still applies**
	- If the `SameSite` attribute on the cookie is set to `Lax` or `Strict`, the browser will not include the cookie in the cross-origin HTTP request, even if you set `Access-Control-Allow-Credentials: true`
	- Learn more about the [`SameSite` attribute for cookies in this article]({{ site.baseurl }}/tutorials/Understanding-Cookies-Part-7/)

---

## Using Allow-Origin + Allow-Credentials
If you're serving a public resource, you can just set `Access-Control-Allow-Origin: *` and be done with it.

However, if you need authentication credentials to also be sent, then you're more restricted in your options.

You can set `Access-Control-Allow-Origin` to a single origin, but that won't work for most situations.

If you need to give access to multiple origins, you will have to use the `Origin` header of the HTTP request to validate and dynamically generate the value for the `Access-Control-Allow-Origin:` header. Be aware though, **this is where security vulnerabilities start to creep in**.

### Recommendations when dynamically generating the origin value
If you must dynamically generate the origin value, be sure that you:
- Don't just reflect back whatever `Origin` you received
- Always use an approved list to check the `Origin` against
- Thoroughly test that your validation for the approved list works properly
- Set the `Vary: Origin` header to prevent cache poisoning attacks and other caching issues
	- See WhatWG's section on [the CORS Protocols and HTTP Caches](https://fetch.spec.whatwg.org/#cors-protocol-and-http-caches){:target="_blank"}

If you don't do these steps properly, you open your users up to even more damaging CSRF-type attacks (because now the attacker can also read the HTTP responses). 

So dynamically generate the origin if you must, but try to do it as safely as possible.

---

## Wrap It Up
CORS is a useful tool for the interconnectedness of the web. But it requires a weakening of the policy that helps keep the web safe - SOP. This means if you use CORS, ensure you implement it correctly to help keep your users safe.

Check out the resources below to help further your knowledge and keep learning so you can better protect your users.

### Further Reading
- [Cross-origin fetches by Google Chrome Developers {YouTube}](https://www.youtube.com/watch?v=vfAHa5GBLio){:target="_blank"}
	- A good discussion and overview of CORS
- [CORS {Mozilla}](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS){:target="_blank"}
- [Fetch Standard (including CORS) {WhatWG}](https://fetch.spec.whatwg.or){:target="_blank"}