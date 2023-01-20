---
layout: post
title: How the Web Works - HTTP Requests & Responses
tags: tech cybersecurity
description: 
thumbnail: /post-thumbnails/2022-07-28-how-the-web-works-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@redaquamedia
thumbnail-creator: Denny Müller
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Icons for the following browsers, Firefox, Google Chrome, Opera, Brave

---

Have you ever wondered what really happens when you click on a link and a webpage magically appears? How does the browser get the data to load the webpage?

At it's foundation, it's actually a really basic concept - HTTP requests and responses. Let's get into it.

<!--more-->

{% include toc.md %}

## What Happens When You Visit a Website
When you visit my website - staciefarmer.com - there's 2 very basic things happening: 
1. An HTTP request gets sent by the browser
2. An HTTP response gets sent by the web server. 

### 1. HTTP request
When you click on a link to visit staciefarmer.com, your browser creates and sends an HTTP request to the server that hosts my website. 

In its most basic form, an HTTP request looks like this:
```
GET / HTTP2
Host: staciefarmer.com
```
Let's go through these 2 lines of this very basic HTTP request.

**Line 1**
- The first word - `GET` - is what method we're using. `GET` means we want a resource from the server. 
	- `POST` is the other method you'll most commonly see and it's used when you want to change the state of data on the web server *(e.g. submit a form that updates a database)*. If you have a POST method, you will also have extra data in the body *(e.g. `username=MyNewName&email=newemail@gmail.com`)* that will be sent to the web server.
	- There are 9 different [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods){:target="_blank"} (Mozilla), but `GET` and `POST` are the most common ones you'll see. 
- The next piece - `/` - is what resource we want. 
	- When it's a single `/`, this means we want the root directory of the website *(also known as the home page)*. 
	- If we want a more specific resource such as the [Understanding Cookies - Overview page]({{ site.baseurl }}/tutorials/Understanding-Cookies-Overview), we would replace `/` with `/tutorials/Understanding-Cookies-Overview/`.
- The last piece of the first line specifies what version of HTTP we're using. In this case we're using version 2, but you'll also commonly see other versions like `HTTP/1.1` or `HTTP3`.

**Line 2**
- This line uses the `Host` header. This specifies the domain name where we can find the root directory. 
	- In this example, our browser is telling the server that we want the home page of the website `staciefarmer.com` *(i.e. `staciefarmer.com` is the domain name)*.
	- We can use multiple request headers. Check out this article for a list of all available HTTP headers - [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers){:target="_blank"} (Mozilla).

There is more to this, like how does the browser know where to send this `GET` request? That's a deeper discussion involving the Domain Name System (DNS). If you'd like to learn more about DNS, you can start with this article from freeCodeCamp - [What is DNS?](https://www.freecodecamp.org/news/what-is-dns/){:target="_blank"}

### 2. HTTP response
The web server for `staciefarmer.com` gets the HTTP request from your browser. Now what?

Well, the web server looks for the resource you requested - the `/` directory *(i.e. the homepage)*. If it finds it (or even if it doesn't), the web server will send an HTTP response back to your browser.

If the web server found the homepage of `staciefarmer.com`, a *very* basic HTTP response might look like this:
```
HTTP/2 200 OK
Content-type: text/html; charset=utf-8

<!DOCTYPE html>
<html>
<head>
...
```
The first 2 lines show a very, very basic HTTP response. Usually you will see many more headers than this, but I wanted to start you off with a very simple example.

**Line 1**
- The first piece specifies what version of HTTP was returned. 
	- Luckily, we asked to communicate over HTTP2 and the server was able to do that. 
	- Sometimes a server may not understand a newer version of HTTP. In that case, it will often downgrade the response to something lower *(e.g. downgrading from HTTP/2 to HTTP/1.1)*.
- The second section is the status code. It tells the browser how the search for the resource went. 
	- In this case, `200 OK` means the web server found the resource and is returning it below. 
	- You can find a list of all the status codes here - [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status){:target="_blank"} (Mozilla).

**Line 2**
- The second line uses the `Content-type` header to tell the browser what format and character set this content is in. This helps the browser display it properly.
	- In this example, the browser is returning some `text/html` content that uses the `utf-8` character set.
	- Just like with the HTTP request, the web server can use multiple response headers. Check out this article for a list of all the available HTTP headers - [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers){:target="_blank"} (Mozilla).

**Line 4-The End**
-  After all the response headers, the web server will include the resource data. In our case, we asked for the homepage of `staciefarmer.com`, so the web server gave the browser the HTML code to build that page.
	- The `...` in our example means there's a lot more HTML code than this. It's just too much to include in our example.
	- *Note:* Some HTTP responses will not have a body

### Viewing requests and responses in your browser
It's one thing to see a very basic HTTP request and response. It's a lot more fun to see real live HTTP requests and responses when you visit a website.

Luckily, many browsers give you the ability to see the requests and responses happening when you visit a web page.

#### For Firefox users
If you're using Firefox: 
- You can right-click on the page and click `Inspect`. 
- Once the developer tools pop-up, you can click on the `Network` tab. 
- When you reload the page, you'll be able to see the HTTP requests and responses being sent and received. 
	- And if you select one of them, you can click the `Raw` selector to see the actual text of the HTTP request or response.

To learn more, visit this article about [using Firefox's Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html){:target="_blank"}.

## What Causes an HTTP Request?
When I learned about HTTP requests and responses, I always wondered - when does the browser know to send an HTTP request?

Ultimately, it comes down to what the user does.

If the user does any of these things, an HTTP request will be sent:
- Clicking a link
- Submitting a form
- Clicking on an element that signals to JavaScript to take one of those actions above or perform a redirect *(i.e. send you to a different webpage)*

These are all fairly noticeable as a user because when they happen, the page reloads.

However, there are ways to send HTTP requests that won't trigger a page reload. These are called Asynchronous requests.

### Asynchronous requests

Asynchronous means doing multiple things at the same time. Even though the browser has already loaded the webpage, that doesn't mean it stops doing things. As long as you've got a browser tab open, the browser is waiting for you, or JavaScript code in the web page, to do something.

If something happens (also known as an "event"), the browser responds.

For example,
- You could click on a button that triggers JavaScript to send a form
- After the entire page is loaded, JavaScript could trigger an event to fetch another resource
- JavaScript could go fetch and load another video ad after the current one finishes playing

These are all examples of events that happen asynchronously. They don't require a reload of the page. They just happen in the background and you can still see them on the Network Monitor (or similar tool for other browsers).

It's a fairly nifty feature.

### What's a client?
One last thing I want to touch on is the term 'client'.

If you delve into HTTP a bit more, you'll see this term a lot*.

The client is the software that sends an HTTP request and receives the HTTP response.

For most users, **a client is their web browser**.

But there are lots of clients out there. You can use command line tools such as cURL as a client. Web apps use libraries to create HTTP clients so they can talk to other websites. Even non-web apps (like desktop software) can have HTTP clients to talk to other web apps.

So, the most commonly used client is a browser, but anytime you talk with a website you're using some form of an HTTP client to create HTTP requests and process the HTTP responses for you.

---

## Wrap It Up
To summarize, we can view websites because our browser, and other HTTP clients, send an HTTP request for us. They get an HTTP response back from the web server. If we're using a browser, it'll take all that HTML code it gets back and make it look nice and pretty for us.

But under the hood, it's simply HTTP requests and HTTP responses.