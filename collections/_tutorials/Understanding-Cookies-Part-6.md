---
layout: post
title: Part 6 - Cookie Attributes Secure & HttpOnly
order: 7
tags: understanding-cookies
description: 
date: 2021-12-14
updated:
---

{% include toc.md %}

In [Part 5]({% link _tutorials/Understanding-Cookies-Part-5.md %}), we discussed how the `Path` attribute changes which paths the browser will share your cookie with.

In this post we'll talk about how the `Secure` and `HttpOnly` attributes work to increase the privacy of your cookies.

---

## `Secure`

Anything sent over `HTTP` is sent in plaintext. This means anyone watching your user's network traffic can view _everything_ sent in the HTTP requests and responses - including cookie details.

`HTTPS` is the more secure version of `HTTP`. This protocol encrypts data, including user's cookies, and prevents spies from viewing and/or modifying your user's network traffic.

If you want to restrict your cookies to only be sent over `HTTPS`, use the `Secure` attribute, like so:

```
Set-Cookie: cookieName=cookieValue; Secure;
```

Best practice is to use `HTTPS` everywhere across your website - especially if you're using cookies. As a precaution though, you should set the `Secure` attribute for all of your sensitive cookies so they will _only_ be sent over `HTTPS`.

### But we use `HTTPS` everywhere. Why do we need `Secure`?

Even if you think you use `HTTPS` everywhere, there could be places where you  don't. `Secure` provides you an extra layer of protection. 

There's also a chance an attacker could perform a downgrade attack on your users. This is when an attacker forces the browser to use `HTTP` or a weak version of `HTTPS` they can crack. Now the attacker can view and possibly modify everything passing between that user and your web server, including cookies.

If you set `Secure` on your sensitive cookies, a user caught in a downgrade attack will no longer have cookies sent over the network. If there are no cookies sent, the attacker can't view or use them to compromise that user's account.

## `HttpOnly`

In [Part 5]({% link _tutorials/Understanding-Cookies-Part-5.md %}), we discussed an attack where JavaScript was used to read the cookie.

To prevent that attack, and others like it, you should set the `HttpOnly` attribute, like so:

```
Set-Cookie: cookieName=cookieValue; HttpOnly;
```

This will only allow the cookies to be accessed through an HTTP request. Attacks using JavaScript or the client's hard disk will not have access to user cookies.

`HttpOnly` is an excellent mitigation tool and should be implemented if possible.

## Additional security concerns

Cookies can be modified by a user or by a malicious actor in transit. For this reason, they should be considered untrusted user input and handled accordingly.

**Don't:**

- use them to store server-side secrets. 

**Do:**

- sanitize and validate cookie values before working with them.

---

**_Next:_** Read [Part 7 - Cookie Attribute: `SameSite`]({% link _tutorials/Understanding-Cookies-Part-7.md %})