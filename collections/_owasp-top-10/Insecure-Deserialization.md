---
layout: post
title: OWASP Top Ten - Insecure Deserialization
tags: tech
description: 
thumbnail: /8-insecure-deserialization/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@mbaumi
thumbnail-creator: Mika Baumeister
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Comma separated numbers printed on a page
date: 2021-06-02

---

Insecure deserialization is a potentially very damaging attack for web applications and it's becoming more common. Let's talk about what it is and how you could try to prevent it.

<!--more-->

{% include toc.md %}

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## What is Insecure Deserialization?
Insecure deserialization occurs when **an attacker can inject their own code into a serialized object which will then be deserialized**.

This could lead to something as dangerous as a [Remote Code Execution (RCE) attack](https://en.wikipedia.org/wiki/Arbitrary_code_execution){:target="_blank"}.

To fully understand deserialization, first you need to understand what serialization is.

### What is Serialization?
Serializing takes a multifaceted item, like an object, and compresses it into a single stream of information.

For example, let's say we have a post object with the following values:
```
post.date = 2021-01-01
post.title = 'Insecure Deserialization'
post.comments = ['1':'truly amazing', '2':'I had no idea']
```

In this post object we have at least 3 different pieces of data - a date, a title, and an array of 2 comments.

Using JSON, we can serialize this data so it can easily be stored or transmitted:
```
{
	"post" : [{
		"date" : "2021-01-01",
		"title" : "Insecure Deserialization",
		"comments" : [
			"1" : "truly amazing",
			"2" : "I had no idea"
		]
	}]
}
```

To put it on one line, in true serial fashion:
```
{"post":[{"date":"2021-01-01","title":"Insecure Deserialization","comments":["1":"truly amazing","2":"I had no idea"]}]}
```

Now we can easily store this data or send it to another part of the codebase.

### What is Deserialization?
After we've serialized our data, we probably want to deserialize it so we can break it back up into individual pieces and do different things with them.

Essentially we're just reversing the previous process. **We read through each piece of serialized data, break it into logical pieces, and do something with them**.

For example, I can take the date and title of the post and put them in a table:
```
post.date => {store in a table}
post.title => {store in same table}
```

Then I might want to loop through each comment, but I'll probably store them in a separate comments table.

This process, of breaking up each piece, is deserialization. This is where we can run into trouble.

---

## Insecure Deserialization Risks
The risk with insecure deserialization happens **if you accept untrusted input - that an attacker could have modified - and deserialize it**. 

You're taking this untrusted input, breaking it up, and using it in your code.

An attacker can use this fact to insert their own malicious code into your serialized data and give you malicious input.

#### Example of a Simple Attack
A common example of this is using a simple cookie for authentication.

Let's say you use the following serialized data in a cookie to authenticate a user after they've logged in:
```
username=monkeylover; lastLogin=1622582700; perm=af37d08ae228a87dc6b265fd1019c97d;
```

The `username` is monkeylover. The `lastLogin` is a timestamp of when this user last logged in. The `perm` is an MD5 hash of their permission level (which is not a recommended way to authorize users, but we'll just go with it for this example).

In this case, their permission level is 'regular'. Nothing special, just normal access. The higher level of access is only permitted for users with a `perm` of 'admin'.

If this is really how you check the authorization level of your application, an attacker could log in with a regular account then change their `perm` value to an MD5 hash of 'admin' like so:
```
username=monkeylover; lastLogin=1622582700; perm=21232f297a57a5a743894a0e4a801fc3;
```

If you're only check the `perm` value to authorize users, then this attacker now has admin access to your website. Great for them. Crappy for you.

Granted, this is a **very simplistic** example. Most insecure deserialization vulnerabilities are much more complicated than this.

This is simply to illustrate that if you are using serialized objects than an attacker can access/manipulate, then you may have an insecure deserialization vulnerability on your hands.

---

## Testing for Insecure Deserialization
Unfortunately, because this vulnerability is very custom to your application, **manual testing is the best way to find insecure deserialization vulnerabilities**. 

They can also sometimes be found using penetration testing tools such as [BurpSuite](https://portswigger.net/burp){:target="_blank"}.

---

## Possible Ways to Help Prevent Insecure Deserialization
[According to OWASP](https://owasp.org/www-project-proactive-controls/v3/en/c5-validate-inputs){:target="_blank"}, the best way to prevent this vulnerability is to **not accept serialized objects from untrusted sources** or to only deserialize in very limited situations using only simple data types.

Whenever possible, **developers should avoid using user-controlled input in their code**.

When that's not possible, OWASP also recommends:
- Using integrity checks or encryption on serialized objects to prevent tampering
- Enforcing strict type constraints during deserialization before object creation
	- _Be aware that there have been demonstrated bypasses to this technique_
- Isolating code that deserializes in very low privilege environments
- Logging security deserialization exceptions and failures
- Restricting or monitoring incoming and outgoing network connectivity from containers or servers that deserialize
- Monitoring deserialization and alerting if a user deserializes constantly

As you can see from the prevention tips, **this is a challenging vulnerability to prevent if you must use deserialize untrusted objects**.

---

## Sum It Up
Deserializing is risky business.

Your best bet is to avoid deserializing untrusted serialized objects.

If you can't do that, do what you can to help prevent tampering by attackers and have logging and monitoring tools in place to catch attacks that slip through.

Like everything with security, insecure deserialization is complex. Check out the resources below to help further your knowledge and keep learning so you can build customized solutions that work for your situation.

#### Further Reading
- [OWASP Top Ten #8 Insecure Deserialization](https://owasp.org/www-project-top-ten/2017/A8_2017-Insecure_Deserialization){:target="_blank"}
- [OWASP Top Ten Proactive Controls 2018](https://owasp.org/www-project-proactive-controls/v3/en/c5-validate-inputs){:target="_blank"}