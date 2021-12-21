---
layout: post
title: You Already Use 2FA Every Day
tag: cybersecurity
description: You've heard you need to set up multi-factor or two-factor authentication. But what is it and how does it work? What if I said you already use it every day? Find out how you use it and why you need to start using it everywhere online.
thumbnail: /post-thumbnails/2019-02-21-you-already-use-2fa-every-day-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@luferlex
thumbnail-creator: Lukenn Sabellano
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Picture of a smartphone with fingerprint scanner
---



You’ve probably heard the terms “multi-factor authentication” and/or “two-factor authentication”. But what are they? Why do you need them? And why are they suddenly such a big deal?

<!--more-->

{% include toc.md %}

Well, **you already know what multi-factor authentication (MFA) is**. You use it in your daily life. When you make a purchase with a credit card they’ll often ask for a signature and maybe your ID. When you make a purchase online, they need multiple things like your name, credit card number, CVV, expiration date, and billing address. When you want to go to an R-rated movie or buy alcohol, and you look young enough, they’ll ask to see your ID.

These are all forms of multi-factor authentication (MFA). **You provide multiple forms of info to prove you are who you say you are**.

---

## The 3 Forms of Authentication
Essentially, there are 3 forms of authentication you can provide. They are:

- **Something you know**
- **Something you are**
- **Something you have**

**“Something you know”** is information you can provide to verify yourself. You can give them your name, address, date of birth, etc. Online you can provide your username and password or answer a “security” question. For debit card purchases you enter your pin.

**“Something you are”** is a part of your body that can be scanned to verify you. Someone can look at your face and match it to your ID. You can have your fingerprint, iris, or face scanned and compare it with a database.

**“Something you have”** is anything you have in your possession that could verify you. Your ID is a very common one. Driver’s license, passport, social security card, or a state ID can all be used to verify your identity. The key to your house or car is something you have. Your phone can also be used to authenticate/verify you.

---

## Why Do I Need More Than One?
Using any one of these factors/forms of authentication can verify you are who you say you are. But **using just one form is not secure**.

Anyone can steal the key to your house or car. IDs can be faked or stolen. People who look similar can impersonate you. Usernames and passwords are leaked all the time and credit card theft is rampant.

While one factor of authentication can get the job done, we generally want something more secure. To do that, **we combine 2 *separate* factors/forms of authentication**. That makes it much harder for the bad guys to get in.

---

## Why *Separate* Forms of Authentication?
Having 2 forms of authentication makes sense. If 1 is good, 2 is better, right? But why does it matter that we have 2 **separate** forms of authentication?

**Because each form of authentication has its weaknesses**. And if you’re using 2 factors from the same group, their weaknesses are similar.

Let’s say you have 2 different keys to open your front door. That’s 2 forms of authentication. If one key gets stolen, the other one probably gets stolen too. That’s because you’ll likely keep those keys in the same pocket or on the same keyring. It’s not difficult to take both of them at once. The same attack can be used to steal both forms of authentication because they have the same weakness.

So you combine your house key with a security code. The key gets you in the front door and the code must be entered to turn off the security system. That’s “something you have” and “something you know”. If someone steals your key, they’re unlikely to also steal your code….unless you wrote it down and kept it with your keys. That’s not a good idea, by the way. By writing it down you’ve changed it to “something you have” instead of “something you know”, which means it can be stolen along with your keys.

But, you didn’t write it down. You memorized it. So a thief needs to steal your key, plus figure out your security code to gain access to your house.

**Two *separate* forms of authentication are better than one**. And three separate forms are better than two. But security needs to be convenient, otherwise you wouldn’t do it. So for most situations, having two separate forms of authentication is decently secure.

---

## Why Does 2FA Matter Online?
**It’s vital that you use 2 *separate* forms of authentication for online accounts**. Because it’s easy to impersonate someone online. It’s easy to trick the computer into thinking a bad guy is you, the good guy. The way the internet works means security is extremely important and also difficult to implement. So businesses have to rely on you, the user, to make things more secure.

Take most online accounts. You need a username and password, right? Those are both “something you know”. What if a bad guy browses one of the many collections of leaked credentials (*usernames with passwords*) and finds yours? Now the bad guy has your username and password. They log into your account and lock you out. They own it. They can do whatever they want until someone figures out what happened, which could take hours, days, weeks, or even months.

Can’t the computer stop them? Yes, but there are many ways to fool the computers. 

**The best way to stop them is for you to add another, *separate*, form of authentication.**

---

## 3 Types of Online 2FA
So, in addition to your username and password, you need to set up a 2nd factor of authentication. **If you have the option, you can purchase and connect your account to a Yubi key or other “physical token”**. It’s a lot like a key fob, except this one has a USB connection. You log into your account, then insert the USB key or press a button on it to wirelessly send a code to the computer. This code is verified and you’re allowed to access the account.

Or maybe you don’t have a Yubi key, or can’t use one with that website. **Your next best option is  an authentication app on your smartphone**. After you log in, you open the app, find the code for that website, and type it in. The code is verified and you’re in.

Well, what about the websites that don’t allow an authentication app (shame on them) or the people who don’t have a smartphone? **Then your best option is to use SMS/text messaging (*preferably with a VOIP number like Google Voice*) as your 2nd factor**. After you log in, a code will be texted to you, and you type it in. That code is verified and you can access the account. 

I know this seems like a lot of information, but it’s just 3 different ways to implement a 2nd factor of authentication.

It’s like the options for disarming a home security system. You can have a static code that you always enter in. It’s easy, but there are downsides. If anyone ever got the code or figured it out, they could easily disable your security system. If you forgot the code, you’d have to call the security company and re-verify yourself to disarm the system.

Or you could use an app on your smartphone to disarm it. What if someone steals your phone? Now they have the capability to disarm your security system. 

You could buy a key fob from the security company. If that gets stolen, then someone else can disarm your system.

Just like online, these are 3 different options to implement a 2nd factor of authentication. They all have their pros and their cons, but any one of them can be used to strengthen your home security.

---

## What Should You Do?
**At this point it’s more important that you *simply enable 2FA* than worry about picking the *right* option to use**. Any of them are better than only using 1 form of authentication. So don’t worry about the best option and set up the most convenient option first.

After a while, you’ll get into the habit of using 2FA. That’s when you can start researching the security risks and find a 2nd factor that both works for you and keeps you secure.

**Right now, make a plan to implement 2FA on every online account you can**. It’s a requirement  now. Because it’s too easy for the bad guys to find or figure out your username and passwords and take over your accounts.

Businesses are starting to get on board, but you’ll find that many still don’t offer 2FA. Before signing up with a company, ask if they support 2-factor authentication. If they don’t, and you have other options, pick a company that DOES support it. Tell the company who doesn’t have 2FA why you won’t do business with them. If enough customers prioritize security, businesses will step up and implement 2FA.

**Remember, you’re already using 2FA and multi-factor authentication in your daily life**. Take those same security principles and apply them to your online accounts. Protect them with 2FA, today.
