---
layout: post
title: Part 2 - Cookie Default Behavior
order: 3
tags: understanding-cookies
description: 
date: 2021-12-08
updated:
---

In [Part 1]({% link _tutorials/Understanding-Cookies-Part-1.md %}), we discussed the basics of why we use cookies and how they work.

In this post we'll talk about what happens when a cookie is set without any attributes.

---

## Cookie defaults

Creating a cookie is fairly simple. The web server just sets a name/value pair like this:

```
Set-Cookie: cookieName=cookieValue;
```

If you set a cookie like this, with no attributes, it will:

- only be stored during that session
	- A session is usually active until the user closes the browser application _(not just the tab)_
- only be sent when the user is visiting that specific domain, not a subdomain
	- For example, if the cookie was set by `http://example.com`, the browser will NOT send it with requests to `http://blog.example.com` or any other subdomain
- only be sent when the user is visiting a page within that path
	- For example, if the cookie was set by `http://example.com/account`, then it will be sent when you visit `http://example.com/account/home`
	- However, it will NOT be sent when you visit `http://example.com/blog` because it's a different path
- be sent over HTTP and HTTPS
- be accessible using JavaScript
- be attached to a resource included in a third-party (different) site

---

**_Next:_** Read [Part 3 - Cookie Attributes: `Expires` and `Max-Age`]({% link _tutorials/Understanding-Cookies-Part-3.md %})