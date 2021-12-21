---
layout: post
title: Useful Tools for Brute Forcing a Login Page
tags: cybersecurity featured
description: When practicing your ethical hacking skills, sometimes you need a specific tool for the job. When it comes to brute forcing web logins, I found a free and open source tool that works.
thumbnail: /post-thumbnails/2020-06-23-useful-tool-for-brute-forcing-a-login-page-thumbnail.jpeg
thumbnail-creator-link: https://commons.wikimedia.org/wiki/File:Wfuzz.jpeg
thumbnail-creator: Topapp
thumbnail-site: https://commons.wikimedia.org/wiki/Main_Page
thumbnail-site-name: Wikimedia Commons
image-alt: Screenshot of the Command Line Interface for the Wfuzz tool
---

I’ve been working on a CTF to expand my pentesting skills. Often, the challenge will be to brute force the login page to get access. 

When I first started brute forcing, I would use [Burp Suite CE](https://portswigger.net/burp){:target="_blank"}. It’s a handy tool with lots of features. Unfortunately, I don't have the funds to pay for the Pro version, but there's still some useful features in Burp Suite CE.

You can use it to brute force a login page, but it will take a long time. The free version throttles your logins and this can quickly become a nuisance. Usually, mine starts out with about 10 logins per second but quickly slows down to about 1 login every 5-10 seconds.

If you’re going through thousands of passwords, this is really frustrating. So I decided to look around for a different tool.

<!--more-->

{% include toc.md %}

---

_Any tools I discuss are only to be used in ethical hacking scenarios where you have the **full permission** of any persons and property involved._

---

## THC Hydra

Online, I came across THC Hydra for a good brute forcing tool. If you’re unfamiliar with it, THC Hydra comes standard in Kali Linux or you can get the code from the [Git repo](https://github.com/vanhauser-thc/thc-hydra/releases/tag/v9.0){:target="_blank"}.

With this tool, to brute force an HTTP login you’ll use a format like this:

``` shell
hydra <IP> -l <USERNAME> -p <PASSWORD> http-post-form “<PATH TO LOGIN PAGE>:<PARAMS AND COOKIES>:<ERROR MESSAGE>”
```

The options `-l` and `-p` can vary based on what you’re using. 
You’ll use:

- `-l` when testing one username
- `-L` when iterating through a file of usernames
- `-p` when testing one password
- `-P` when iterating through a file of usernames

The `<ERROR MESSAGE>` is error text that’s displayed on the page when you're unsuccessful logging in. 

If the error message changes based on whether your username or password is incorrect, try to brute force the username to figure it out first, then brute force the password with the known username.

I also like to add a few parameters:

- `-V` for verbose output (so I can see how things are going)
- `-o` <filename> for saving the results to a file 

One other parameter I occasionally use:

- `-f` stop on successful login

This will stop all tries once a successful username/password combination is found. At first, I thought this was a great option, until I realized how finicky hydra can be with HTTP forms.

With a particular web application, I had figured out the username and was trying a list of 10k passwords. I tried the hydra command:
	
``` shell
hydra 127.0.0.1 -l admin -P passlist.txt http-post-form “/login:username=^USER^&password=^PASS^:Invalid password” -V -o hydra-output.txt -f
```

2 different times I ran it and got at least 3 different “successful” logins - meaning Hydra had found 3 separate passwords that would work with the username. I manually checked each password with my known username and each was wrong.

I discovered that hydra has a problem with false positives when brute forcing HTTP - it apparently works better with FTP and Telnet. So, I removed the `-f` option and tried it again. After testing a 100k passwords, I still hadn’t found the right one. This seemed odd to me, so I decided to go searching for a different tool.

## Wfuzz

Wfuzz is also a brute force authentication tool, but is specifically designed for web apps. Like THC Hydra, it’s free and included in Kali Linux - or you can download the source code from the [Github repo](https://github.com/xmendez/wfuzz/){:target="_blank"}

To brute force a login with HTTP, you’ll use this basic syntax:

``` shell
wfuzz -c -w <FILE> -d “<PARAMS>” —hs <ERROR TEXT> <URL>
```

Already you can see how much simpler wfuzz is for HTTP than hydra.

The options I included above are:

- `-c` outputs with colors (_an enhancement I like_)
- `-w` lets you specify a wordlist to use (_with `<FILE>` being the path to the wordlist file_)
- `-d` is the post names and values you’ll be sending. For the value you’re fuzzing, replace it with FUZZ (_see the command I used below_)
- `—hs` will hide responses with the `<ERROR TEXT>` values (_helpful so you don’t have to see all the unsuccesful tries_)

A big thanks to [Pete at SecurityBytes for his tutorial on using wfuzz](https://securitybytes.io/wfuzz-using-the-web-brute-forcer-1bf8890db2f){:target="_blank"}. His tutorial helped me understand how to use wfuzz and adapt it to my needs.

In my case, I knew the username (let’s say it was ‘admin’ for this example). The login URL was http://example.com/login (also not the real URL). The error message for a wrong password was ‘Invalid password’.

With that info, the command I used looked something like this:

``` shell
wfuzz -c -w /usr/share/seclists/Passwords/password-list.txt -d “username=admin&password=FUZZ” —hs “Invalid” http://example.com/login
```

After a few thousand passwords, it found the right one! I verified it worked and got my flag. It was great to finally find a tool that worked perfectly for the job - and was free.

If you find yourself needing to use wfuzz, there are a lot of options for customizing it. Check out [Wfuzz's super helpful docs](https://wfuzz.readthedocs.io/en/latest/user/basicusage.html){:target="_blank"} and I also recommend checking out the [Security Bytes tutorial](https://securitybytes.io/wfuzz-using-the-web-brute-forcer-1bf8890db2f){:target="_blank"} as well.
