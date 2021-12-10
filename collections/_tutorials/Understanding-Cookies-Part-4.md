---
layout: post
title: Part 4 - Cookie Attribute Domain
order: 4
tags: understanding-cookies
description: 
date: 2021-12-10
updated:
---

{% include toc.md %}

In [Part 3]({% link _tutorials/Understanding-Cookies-Part-3.md %}), we discussed how the 2 attributes, `Expires` and `Max-Age`, alter how long a browser will store your cookies.

In this post we'll talk about how the `Domain` attribute changes which domains the browser will share your cookie with.

---

## `Domain`

By default, a browser will only send cookies to the specific domain that set the cookie - not any of its subdomains. This is generally the more secure option.

If you want the browser to share your cookie with a specific domain and all of its subdomains, you can set the `Domain` attribute like so:

```
Set-Cookie: cookieName=cookieValue; Domain=example.com;
```

In this example, the browser will attach the cookie to all requests for `example.com` and its subdomains, such as:

- `blog.example.com`
- `admin.example.com`
- `reports.admin.example.com`

**_Note:_** Setting the attribute as `domain=example.com` or `domain=.example.com` is considered identical and acts the same.

If you want the cookie to be sent to a specific subdomain, such as `blog.example.com`, and all of its subdomains, (like `test.blog.example.com`), you would set it like this:

```
Set-Cookie: cookieName=cookieValue; Domain=blog.example.com;
```

### Security concerns

**Warning!** Subdomains are an attack vector and should not be implicitly trusted. By setting the `Domain` value, you are making your cookie less secure.

For example, let's say an attacker can run code on your subdomain `test.example.com` using a [Cross-Site Scripting (XSS) attack](https://owasp.org/www-community/attacks/xss/){:target="_blank"}.

You set your log in cookie with the site declared as the domain, like so:

```
Set-Cookie: authorized=LargeRandomNonce; Domain=example.com;
```

This cookie will now be shared with all the subdomains for that site. If an attacker can trick a logged in user to visit the subdomain they control (`test.example.com`), that user's cookie will be shared with the attacker. They can then use that cookie to gain access to the user's account. Not ideal.

If you'd like to keep your cookies more secure and prevent them from being shared between subdomains, you should NOT set the `Domain` attribute.

**_Note:_** If you host user content, instead of using subdomains, consider moving user hosted content to a different domain. For example, GitHub allows you to host your own website, but not using `github.com`. All user content is hosted on `github.io` - a completely different site. This helps GitHub avoid having untrusted user content available on their main domain and reduces the likelihood of subdomain attacks. 


### Can I set the domain from a subdomain to a higher level domain?

Not directly, because the site you are specifying (the other, non-inherited subdomain) does not include the origin server.

**What's an origin server?**

The [origin server](https://developer.mozilla.org/en-US/docs/Glossary/Origin){:target="_blank"} is the scheme (protocol), hostname (domain), and port of the URL this request came from.

You can't set the `Domain` for a different subdomain from a non-inherited subdomain because they do not share an origin.

For example, let's say a user logs in on your subdomain - `https://login.example.com`. The origin server would be `https://login.example.com:443` (where the port is implied by the `HTTPS` protocol).

But you want to set the cookie so it can only be used on a different subdomain - `account.example.com`. 

If you were to set `Domain=account.example.com`, the browser would just ignore the cookie and not store it. Why? Because `account.example.com` is not included in the origin server - `https://login.example.com:443`.

You'd have to set `Domain=example.com` first, then it can be accessed by ALL the subdomains of that site, which is a potential security risk. But you can't directly set the `Domain` for a subdomain from a different, non-inherited subdomain.

### Can I set a cookie for someone else's domain?

You can try, but it won't work.

If you set a cookie from your web server, let's pretend it's at `https://example.com`, but declare the domain to be Yahoo, like this:

```
Set-Cookie: cookieName=cookieValue; Domain=yahoo.com;
```

The browser will just ignore your cookie and not store it. Why? Because `yahoo.com` is not part of the origin server - `https://example.com:443`.

---

**_Coming Soon:_ Part 5 - Cookie Attribute: Path**