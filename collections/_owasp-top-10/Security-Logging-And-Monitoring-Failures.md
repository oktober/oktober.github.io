---
layout: post
title: OWASP Top Ten - Security Logging & Monitoring Failures
tags: tech
description: 
thumbnail: /10-insufficient-logging-and-monitoring/thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@techdailyca
thumbnail-creator: Tech Daily
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: A laptop with a graph showing trends over time
date: 2021-06-15
  
---
*Previously known as 'Insufficient Logging and Monitoring'.*

Attacks are happening all the time, but would you be able to recognize one as it's happening?

<!--more-->

{% include toc.md %}

Most of us wouldn't, which is why OWASP identified "Security Logging and Monitoring Failures" as #9 in their [2021 Top Ten Web Application Security Risks](https://owasp.org/www-project-top-ten/){:target="_blank"}. It's a vulnerability we could all use a little help preventing.

---

_All examples of potential attacks in this article are for demonstration and educational purposes only. They should never be used outside of a lab environment or to harm other computers, users, etc._

---

## Security Logging and Monitoring Failures
Logging allows us to get an idea of what's going on with our application. Monitoring shows us what's happening in real, or near-real, time.

Too often though we're not logging or monitoring or we're doing it, but not very well.

Unfortunately, this deficit makes attacks more likely to happen and more devastating when they do. Attackers can occupy our systems for longer, do more damage, and cause more harm.

---

## Possible Ways to Help Prevent Security Logging and Monitoring Failures
Logging and monitoring solutions will be unique to your specific situation, but the following list, compiled from [OWASP's recommendations](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/){:target="_blank"}, is a great place to start to determine if your current process might be lacking and ways you could improve it.

### 1. Log Appropriately 
- Use a common log format across systems so you can easily feed it to an aggregator
- Ensure timestamps are consistent across systems
- Log only as much as you need and avoid logging sensitive info
	- Try to log just enough user context so you can identify suspicious or malicious accounts
- Encode & validate dangerous characters to help prevent log injection attacks
- Hold logs for a sufficient time to allow delayed forensic analysis

### 2. Prevent Tampering 
- Forward logs from distributed systems to a central, secure logging service
	- This allows for centralized monitoring and can help prevent lost data
- Evaluate permissions of log files and log changes
- Ensure high value transactions have an audit trail with integrity controls to help prevent tampering or deletion

### 3. Respond Timely and Appropriately
- Preferably, have an automated response in place to react to possible attacks in real time
	- If that's not available, determine how you can easily notify the appropriate people and respond to an attack in near real time
- Analyze your threshold for alert response and escalation
- Establish an incident response and recovery plan

---

## Sum It Up
Proper logging and subsequent monitoring can be a valuable tool in detecting attacks and reducing their severity. However, it's a complex process that's unique to every application and organization. 

Check out the resources below to help further your knowledge and keep learning so you can build customized solutions that work for your situation.

#### Further Reading
- [OWASP Top Ten #9 Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/){:target="_blank"}
- [OWASP Proactive Controls: Implement Logging and Monitoring](https://owasp.org/www-project-proactive-controls/v3/en/c9-security-logging.html){:target="_blank"}
- [OWASP Cheat Sheet: Logging](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html){:target="_blank"}