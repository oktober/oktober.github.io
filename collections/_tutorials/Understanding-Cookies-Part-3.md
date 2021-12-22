---
layout: post
title: Part 3 - Cookie Attributes Expires & Max-Age
order: 4
tags: understanding-cookies
description: 
date: 2021-12-09
updated:
---

{% include toc.md %}

In [Part 2]({% link _tutorials/Understanding-Cookies-Part-2.md %}), we discussed how a cookie behaves when no attributes are set.

In this post we'll talk about how the 2 attributes, `Expires` and `Max-Age`, alter how long a browser will store your cookies.

---

## `Expires`

By default, browsers delete cookies when the browser application is closed.

To ask the browser to store your cookie until a future data and time, you would set the `Expires` attribute like so:

```
Set-Cookie: cookieName=cookieValue; Expires=Wed, Jan 31 2021 22:30:00 GMT;
```

## `Max-Age`

If you want the browser to store the cookie for a specific amount of time (represented in seconds), you would set the `Max-Age` attribute like so:

```
Set-Cookie: cookieName=cookieValue; Max-Age=86400;
```

This cookie would be kept by the browser for 24 hours _(24 hours * 60 minutes * 60 seconds = 86,400 seconds)_.

**Remember!** You are merely asking the browser to store your cookie until a certain date and time or for a certain amount of time. If the browser needs to free up space or thinks your `Expires`/`Max-Age` is unreasonable, it can delete your cookie.

## Older vs newer browsers

Older browsers might not recognize the newer `Max-Age` attribute. If you need backwards-compatibility, set both `Expires` and `Max-Age`.

If you set both attributes, newer browsers will always use the `Max-Age` value to determine how long to store your cookie.

---

**_Next:_** Read [Part 4 - Cookie Attribute: Domain]({% link _tutorials/Understanding-Cookies-Part-4.md %})