---
layout: post
title: When analytics becomes intelligence
date: 2026-04-13 13:37:00 +0100
description: Citrix usage tracking by Pendo.io and its hidden risks
img: header/citrix-pendo-risks.gif
tags: [citrix, pendo, analytics, social-engineering, privacy]
---

## The hidden layer

Starting with version 2411, Citrix Web Studio integrates [Pendo}(https://www.pendo.io/about/) for usage tracking to understand how administrators interact with the management console, with the feature **enabled by default**.

Pendo captures data on feature clicks, page loads, and user interactions to provide insights into user behavior and improve the product experience. As per default this information is stored on the US geo-located GCP (Google Cloud Platform).

In many, cloud-based and even on-premises, Citrix environments, the Pendo.io analytics tooling is quietly embedded (a.k.a. as 'opt-out feature') into the interface. Its purpose is harmless on paper:

- track feature usage 
- improve UX 
- guide users 

Even when your Citrix environment is in a secluded (no proxy or internet connectec) your workstation, on which you do the administrative tasks, is most probably connected to the interwebs. The API calls, while using the Citrix administrative tools mentioned, are instigated from your workstation towards Pendo.io, over **your** internet connection. 

In practice, it introduces a **second layer of visibility**—one that observes not just systems, but **people using them**.

---

## The token you didn’t notice

Pendo uses a structured token (often referred to as a **JZB token**) to correlate activity, see the [pendo-io JZB tool on Github](https://github.com/pendo-io/jzb).

It typically contains Base64-encoded JSON such as:

- `visitor_id` - Can contain your Active Directory **domain name** and **account name**, possibly [UPN](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/plan-connect-userprincipalname#upn-format) formatted.
- `account_id` - In case of on-premises Citrix, it can contain your **domain** in the URL
- `browser_time` - Time stamp (epoch) of **your activities**
- `loginMethod` - Which **logon method you used**
- `adminType` - What level of **administrative right you have**
- `version` - Which **version** of Citrix is installed
- `numberofDDCs` - This can indicate the **size** of the Citrix environment

This token is:

- readable in the browser 
- accessible to scripts 
- transmitted with analytics events 

It doesn’t grant direct access—but it answers an important questions:

> *In case of UPN naming: what is **your real name** for [OSINT](https://en.wikipedia.org/wiki/Open-source_intelligence)?*
> *Who is doing what action?*
> *What is your level of administrative rights?*
> *Which hours and days are you working?*
> *Which company are you working for?*
> *What kind of environment is installed?*

---

## Behavior is the real data

Now combine that identity context with what Pendo actually tracks:

- user profiling
- customer profiling
- environment profiling
- clicks 
- navigation paths 
- feature usage 
- workflow/hours timing 

You no longer have telemetry.

You have: 

> **a behavioral blueprint of your (administrative users in your) organization**

---

## ~~If~~ When this data leaks

Even without a breach of core systems, exposure of analytics data enables:

- mapping of internal tools 
- identification of high-privilege users 
- reconstruction of workflows 
- timing of user activity 

An attacker doesn’t need credentials to learn:

> how your system is used in the real world

---

Instead of relying on generic phishing campaigns, access to detailed platform insights enables highly targeted and convincing attacks—often referred to as **spear phishing** or even **context-aware social engineering**.

With this level of information, attackers can significantly increase their success rate by aligning their approach with the victim’s real environment and behavior:

* Highly specific target audience
* Messages can be tailored specifically to administrative or privileged users of a platform, who typically have broader access and higher impact if compromised.
* Contextual credibility through workflow references
* By referencing actual processes, tools, or internal terminology, attackers can craft messages that feel legitimate and familiar, lowering suspicion.
* Convincing replicas of real interfaces
* Fake login pages or task flows can closely mimic the actual platform, making it difficult—even for experienced users—to distinguish between real and malicious interactions.
* Behavior-based timing
* Delivering phishing messages at moments when users are most active (e.g., start of workday, routine task windows) increases the likelihood of engagement and reduces critical scrutiny.

**Why this is more dangerous?**

Unlike traditional phishing, this approach reduces the typical **red flags** users rely on. The attack blends into normal operations, exploiting trust, routine, and familiarity rather than just curiosity or urgency.

**Practical implication**

This means that user awareness alone is no longer sufficient. There is a need for layered defenses, such as:

* Strong authentication (e.g., MFA)
* Behavioral anomaly detection
* Strict access controls
* Continuous monitoring of unusual workflows or login patterns

---

## When the damage is already done, how to remove the data from Pendo.io?

1. Plug the leak

Disable the Pendo snippet or tracking, see section 'How to disable...' below.

Automate where applicable.

2. Request for deletion existing data

Contact Citrix/Pendo support for:
Visitor/account deletion
Bulk data removal

3. Require proof of deletion

**Bottom line**: Stop collection first, then request backend deletion—prevention is key going forward.

## How to disable or limit Pendo

### 1. Block at network level

Block DNS requests for:
- *.pendo.io

### 2. Browser blocking

- Use content blockers or policies

### 3. Citrix settings

- Disable analytics / telemetry in admin consoles, see:

    [Citrix Web Studio - disable Pendo](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure/install-core/install-web-studio.html#optional-enable-or-disable-pendo)

    [Citrix Director - disable Pendo](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/director.html#usage-data-collection-by-pendo)

### 4. Verify

- Check DevTools (F12) → check for **pendo.io** calls

---

## Final thoughts

> If data can be used to imitate behavior, it becomes intelligence.

> Stay away from the *Citrix Customer Experience Improvement Program* a.k.a. *CEIP*, as far as possible.

> [Richard Stallman is right!](https://www.youtube.com/watch?v=Ag1AKIl_2GM)