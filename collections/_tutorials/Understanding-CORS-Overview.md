---
layout: post
title: Understanding CORS & Misconfiguration Vulnerabilities
order: 3
tags: understanding-cors overview featured
description: 
date: 2022-08-24
thumbnail: /post-thumbnails/2022-08-14-cors-headers-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@yanivcphoto
thumbnail-creator: Yaniv Cohen
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Car crossing a bridge
---

Cross-Origin Resource Sharing (CORS) is a useful tool to carefully relax the security precautions that Same Origin Policy (SOP) provides. It allows you to share your resources with other origins, but it can also introduce new security vulnerabilities into your web app. 

In this series of posts, you can learn more about how to implement CORS on your web server and understand the potential vulnerabilities that can occur.

---

## Prerequisites
Before you start learning about CORS, you should have a basic understanding of: 
- [How the web works - HTTP requests and responses]({{ site.baseurl }}/How-The-Web-Works)
- [Same Origin Policy (SOP)]({{ site.baseurl }}/Same-Origin-Policy)

---

## CORS headers & potential vulnerabilities
Once you understand those basic concepts, you can learn more about:
- [Basic CORS headers for your web server]({{ site.baseurl }}/CORS-Headers)
	- Learn about the `Access-Control-Allow-Origin` and `Access-Control-Allow-Credentials` response headers in depth
- [CORS configuration vulnerabilities - part 1]({{ site.baseurl }}/CORS-Configuration-Vulnerabilities-Part-1)
	- Learn what can go wrong when you dynamically generate the `Access-Control-Allow-Origin` using the `Origin` request value
- [CORS configuration vulnerabilities - part 2]({{ site.baseurl }}/CORS-Configuration-Vulnerabilities-Part-2)
	- Learn about all the other vulnerabilities and risks associated with using CORS headers in your web app