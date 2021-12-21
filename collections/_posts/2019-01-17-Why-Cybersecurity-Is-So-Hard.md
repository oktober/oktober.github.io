---
layout: post
title: Why Cybersecurity Is So Hard
tag: cybersecurity
description: Why is cybersecurity so difficult? It feels like there's new rules to follow every day. There's a new threat. New ways of doing it wrong. It's not you. The systems are broken and always have been.
thumbnail: /post-thumbnails/2019-01-17-why-cybersecurity-is-so-hard-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@etaplus
thumbnail-creator: ETA+
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: An old computer with a smiley face made from pink tape
---

Cybersecurity can be overwhelming. 

Practicing it requires us (users) to develop new habits.

We need to use MFA (multi-factor authentication). And use strong, unique passwords. Definitely don't give out PII (personally identifiable information) - even though EVERY single business asks for way more than they need nowadays.

Those are tough habits to make automatic.

And there's a reason it's difficult to be secure, other than human psychology. 

<!--more-->

{% include toc.md %}

## Why Is Cybersecurity So Hard To Do?
Our world becomes more connected every day. Companies feel entitled to our information without providing adequate security or privacy measures. Software is built to enable ease of use over security.
 
It's not just you - cybersecurity is hard for everyone, everywhere. Because our systems and society refuse to build privacy and security into their foundations. It's not a priority in our society, so it's not a vital part of the creation process. 

We want our products quick and cheap. Security's just not a priority.

### Passwords Are Flawed
Let's talk about the design of using passwords.

Passwords, since the beginning, have  been flawed. 

A password is a static value. It doesn't change often. Someone, somewhere knows it (technically two "people" know it since it takes 2 to authenticate). Someone else can guess it or get access and guess what? It's compromised.

If a malicious hacker tricks you into revealing your password, now it's compromised.

If an attacker spends the time to brute force it, eventually they'll compromise it. If they get access to the database where it's stored, and the hash, eventually they'll decrypt it and know it.

Increases in processing power make passwords easier to crack every year. We always knew that was going to happen. But passwords were a [decent solution for their time](https://www.wired.com/2012/01/computer-password/){:target="_blank"}.

Now, we're stuck with a system that wasn't intended for networks that can be accessed globally. They're not very secure for that. To combat the password's growing vulnerabilities, we stack on additional authentication methods.

### Better, But Imperfect Solutions
So now you can't just have unique, strong passwords (though that's a good start for everyone). You also need MFA (*multi-factor authentication, commonly implemented as 2FA - 2-factor authentication*).

The most convenient way to implement it (*SMS/text messaging*) is also the least secure way. It's much better than just a username & password though, so I'd recommend using it as opposed to nothing.

It works by having you enter your login credentials first. If correct, the company sends you an authentication code over text. You enter this code and you can access your account.

The authentication code is slightly  more secure than just a password. It's not a static value. It will change each time you try to log in, so it's harder to guess. 

But the **3 forms of authentication are:**

1. Something you know (*like your username/password*)
2. Something you have (*like a physical key/token*)
3. Something you are (*like a fingerprint/facial scan*)

Technically an authentication code sent over text message is none of these things. It becomes something you know once it's sent to you. But that's the same authentication type as a password. So, technically it's not 2-factor authentication. Just 1-factor.

But I have the phone, right? Physically, yes, but you're not using the phone itself to authenticate you. 

A code is being sent over cellular to you. It's vulnerable to interception while it's being sent (*like SIM swaps*). 

Even if you get it, you still have to enter the code. It's vulnerable to interception when you enter it (*like a phishing site that tricks you into entering your authentication code on a malicious website, but it looks like the real deal*).

Don't get me wrong, SMS messaging is better than just a username and password - especially with [data breaches becoming the norm](https://www.breachlevelindex.com/){:target="_blank"}.

But we're still cobbling together security methods to try and make the username/password system  more secure. Because of its widespread use, we don't have the option to scrap it and build something genuinely secure from the bottom up.

### Insecure Beginnings
As users, it totally makes sense why we might get frustrated.

The original method (*username & password*) was insecure. So use a stronger (*longer*) password. Oh, but now you have to make sure it's unique for every site and  it needs to be fairly random. 

Guess what? Random isn't as necessary anymore. Just make it long enough so it's difficult to crack. 

But that's not really good enough. Now you also need to enable 2-factor authentication. You could use text messages, but an authenticator app is better and a physical token is much better than either text or the app.

It's constantly changing, because **the foundation (*username & password*) was insecure**. No matter what, we will always be trying to make it more secure. It's exhausting to think about sometimes.

Unfortunately, as [Troy Hunt surmises](https://www.troyhunt.com/heres-why-insert-thing-here-is-not-a-password-killer/){:target="_blank"}, passwords are likely here to stay. So we better get used to it.

### Not Just Passwords
Passwords aren't the only guilty party. The [internet is inherently insecure](https://www.washingtonpost.com/sf/business/2015/05/30/net-of-insecurity-part-1){:target="_blank"}. That's why we have to add things like HTTPS and VPNs. 

At its core, the internet is about connecting with others. To connect, you first have to trust. But when we started connecting, trust was implied, not earned.

Now we've realized trust should be earned first. But that means adding on to the internet's foundations and trying to cobble together a newer, more secure connection. Just like with passwords.

### It Is What It Is
I'm not at all saying I know how to fix it, or even that I would begin to know how to do it better. It's easy for me to sit here, decades after these systems were put in place and point out their faults.

We are where we are. It is what it is.

I point this out because this is partly why cybersecurity is so damn difficult, especially for us users. 

**The systems are inherently insecure**. We're trying to improve them, but replacement isn't an option. The best we can do is improve upon them, but the foundations will always stay the same.

That's why it's hard. It's not just you. It's not just me. It's the systems.

---

## So What Can You Do?
I know it can feel overwhelming, but all you need to do is try your best.

**Learn about cybersecurity**, like you're doing now.

**Find little things you can do to become more secure**.

Get a plan and take one step at a time

1. Get a password manager 
2. Go through and add your accounts
3. Then go through and update your passwords to be strong and unique
4. Then enable MFA where you can
5. Then research other areas where you could be more secure

It's just takes ***one change at a time***.

You can't become secure overnight. And you'll never be 100% secure anyway.

The goal is to make gradual improvements. ***Anything you CAN do is a good thing***.

I know, it kind of sucks. But at least you know it's not your fault. The system is kind of rigged against you. And it'll always be an uphill battle.

But you're fighting the good fight. You're learning and making small changes. Those will add up to a more secure and safe you over time. 
