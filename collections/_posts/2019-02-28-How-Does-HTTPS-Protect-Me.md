---
layout: post
title: How Does HTTPS Protect Me?
tags: cybersecurity featured
description: HTTPS should be used everywhere online, but why? And how does it protect you? Are there ways it doesn't? Learn about how HTTPS works to secure your communications and why it can't protect you from malware.
thumbnail: /post-thumbnails/2019-02-28-how-does-https-protect-me-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@gabewasylko
thumbnail-creator: Gabriel Wasylko
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Picture of locked wall safe
---

HTTPS is misunderstood. People see the little green lock and think, “Great! I’m safe.”

**But HTTPS isn’t about being safe**. Underneath that green lock you’ll see the word ‘secure’. That’s what HTTPS is for, secure communications, not being safe.

<!--more-->

{% include toc.md %}

HTTPS does 2 things. **It verifies you’re talking with the right website and encrypts your web traffic.**

---

## How Does HTTPS Work?
When you visit an HTTPS website, it will take a few seconds to load. Your browser and the web server have to **first establish a secure connection using the TLS handshake.**

The TLS handshake is like greeting a stranger. You make a connection and tell each other information.

First, the browser contacts the web server and says hi. It sends the web server some information  and waits for a response. The web server gets the info and says hi back. It sends the browser information including its X.509 (SSL) certificate. Once the browser gets this, it can verify it’s talking to the right web server. Then the browser sends its last bit of data and says “I’m finished.” The web server gets this data and says “I’m finished too.”

**Everything after this point is encrypted**. The data they shared allows them to create ‘symmetric keys’. These keys are used to encrypt all communication going forward. An eavesdropper will have a hard time figuring out the symmetric keys because of the handshake’s design. 

That extra time to load allows your browser to verify the website and create an encrypted ‘tunnel’ for communication. This prevents eavesdroppers from spying or modifying your communications with that website.

---

## Doesn’t HTTP do this?
**HTTP sends all web traffic in the clear**. Anyone watching the data travel back and forth can read everything. They can also modify it. This is called a MITM (Man-In-The-Middle) attack. **It’s often used to spy, install malware, or steal login information**. 

Sometimes your ISP (Internet Service Provider - where your internet comes from), the ultimate MITM, inserts helpful pop-ups into HTTP websites. They want to tell you how much data you’ve used and they do this by modifying the webpage as it’s sent to you.

**HTTP web traffic can be viewed and modified at any point. HTTPS verifies and secures your communication to prevent a MITM from attacking.**

---

## Then Why Isn’t HTTPS Safe?
HTTPS only ***secures*** the communication between browser and web server. **It can’t ensure the communication is safe**. It was never designed for that.

**Malicious websites use HTTPS to create a false sense of security**. Too many people misunderstand HTTPS and think they’ll be safe. You won’t be. Using HTTPS, you can download malware from the correct website without anyone spying or modifying the malware before it gets to you. But HTTPS won’t stop you from infecting yourself.

Malware is everywhere online. You could be visiting a non-malicious website and still get infected. HTTPS isn’t designed to protect you from it. 

A good antivirus running regular scans can help. Being skeptical about websites you visit and what you click on or download can also help.

**HTTPS verifies and secures our communication, but it can’t keep us “safe” online.**

---

## If I Use HTTPS, Will I Always Be Secure?
I wish. Unfortunately, **there are ways HTTPS can be misused or abused**.

**Website administrators must set up HTTPS properly** and only include HTTPS content in their sites. They have to keep control of their DNS records to keep their X.509 (SSL) certificate safe. If they fail at any of these things, you can still be at risk for MITM attacks.

**Products also exist that eliminate the security of HTTPS**. You’ll see them called ‘content scanning’ or ’SSL Inspection’ tools. Businesses often use them to monitor employees online. The company will install a root certificate on each device to allow all web traffic, including HTTPS, to be monitored and scanned for possible malware. 

These are things you can’t control, but you should still know about them. Now, let’s talk about things you can control.

---

## How Can You Stay Safe Online?
First, you need an **up-to-date antivirus scanning regularly**, if possible. All your **devices and software need to be up-to-date, including browsers**. Avoid pop-ups, even if they seem helpful. If a pop-up tells you to update something, go directly to the website and download the update there. Never install something from a pop-up online.

**Always use HTTPS websites when you can**. If you’re on a public WiFi network, like at a coffee shop or hotel, **use a VPN (Virtual Private Network)**. They are affordable, easy to use, and worth the protection.

---

## Sum It Up
HTTPS verifies and secures your online communications. It protects you from MITM attacks if setup correctly, but it can’t keep you safe from malware. **It’s still the best option and you should use it whenever possible**.

Nothing can be 100% secure. Do what you can to be safe and stay aware. **Use HTTPS along with other secure practices, like using an antivirus, running regular updates, and using a VPN, to protect yourself online**.
