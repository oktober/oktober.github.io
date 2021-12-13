---
layout: post
title: Part 5 - Cookie Attribute Path
order: 5
tags: understanding-cookies
description: 
date: 2021-12-13
updated:
---

{% include toc.md %}

In [Part 4]({% link _tutorials/Understanding-Cookies-Part-4.md %}), we discussed how the `Domain` attribute changes which domains the browser will share your cookie with.

In this post we'll talk about how the `Path` attribute changes which paths the browser will share your cookie with.

---

## `Path`

By default, a browser will only send cookies to the specific path, and its subdirectories, that set the cookie - not any paths above it.

For example, let's say the following cookie is set by `example.com/account/`:

```
Set-Cookie: authorized=LargeRandomNonce;
```

The browser will now send this cookie if the user visits `example.com/account/` and any sub-directories, such as:

- `example.com/account/home`
- `example.com/account/list/`
- `example.com/account/directory/`

The browser will not send the cookie if the user visits a different path for that domain, such as `example.com/blog/`.

However, if you want the path to be different than the default, you can set it with the `Path` attribute.

### Setting `Path`

Generally, you'll find yourself using `Path` when you're in one directory but want to set the cookies for a different directory.

For example, the following cookie is set by the path `example.com/login/`, but we only want the cookie sent when the user visits `example.com/account/` and its sub-directories.

We'd set the cookie like this:

```
Set-Cookie: authorized=LargeRandomNonce; Path=/account/;
```

Now, the cookie will be sent anytime the user visits `example.com/account/` or its subdirectories, such as `example.com/account/edit`.

It will not be sent if the user visits any other directory on the site, such as `example.com/login/` or `example.com/blog/`.

### Don't rely on it for security

Paths might seem a good way to secure your cookies, but there are vulnerabilities you should know about.

For example, let's say an attacker can run code somewhere in `example.com/legacy/` using an [XSS attack](https://owasp.org/www-community/attacks/xss/){:target="_blank"}.

After a user logs in, we set the cookies to only be sent to the `/account/` path like so:

```
Set-Cookie: authorized=LargeRandomNonce; Path=/account/;
```

Cookies will now only be attached to requests for the `/account/` path and its sub-directories.

The attacker can't get the cookie sent to them in an HTTP request because they control a different path. However, they might be able to read that cookie using iframes and Javascript.

For example, the attacker could:

1. create a web page at `example.com/legacy/accountDetails.html`
2. add an iframe to this webpage that loads `example.com/account/userProfile.html`
3. lure a logged in user to visit the malicious webpage (`accountDetails.html`)
4. use Javascript to read the iframe's cookie using `contentDocument.cookie`

_Read more about [this type of attack at mozilla.org](https://developer.mozilla.org/en-US/docs/web/api/document/cookie#security){:target="_blank"}._

This is possible because both the webpage and iframe exist on the same origin (`http://example.com:80`) and we haven't disabled Javascript from reading the cookie.

To prevent this attack, you'd need to add the `HttpOnly` attribute to your cookie, which prevents Javascript from accessing a cookie. We'll discuss this attribute more in Part 6.

`Path` is a useful attribute, but should not be relied on solely for security. It's just one tool that should be combined with others to help increase the usability and security of your cookies.

---

**_Coming Soon:_ Part 6 - Cookie Attributes: `Secure` and `HttpOnly`**