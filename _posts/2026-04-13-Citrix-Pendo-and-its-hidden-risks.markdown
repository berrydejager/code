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

But in practice, it introduces a **second layer of visibility**—one that observes not just systems, but **people using them**.

---

## The token you didn’t notice

Pendo uses a structured token (often referred to as a **JZB token**) to correlate activity, see the [pendo-io JZB tool on Github](https://github.com/pendo-io/jzb)

It typically contains Base64-encoded JSON such as:

- `visitor_id` - Can contain your Active Directory **domain name** and **account name**
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

> *Who is doing what action?*
> *What is your level of administrative rights?*
> *Which hours and days are you working?*
> *Which company are you working for?*
> *What kind of environment is installed?*

---

## Behavior Is the Real Data

Now combine that identity context with what Pendo actually tracks:

- clicks 
- navigation paths 
- feature usage 
- workflow timing 

You no longer have telemetry.

You have: 

> **a behavioral blueprint of your (administrative users in your) organization**

---

## If This Data Leaks

Even without a breach of core systems, exposure of analytics data enables:

- mapping of internal tools 
- identification of high-privilege users 
- reconstruction of workflows 
- timing of user activity 

An attacker doesn’t need credentials to learn:

> how your system is used in the real world

---

## Social Engineering, Upgraded

Instead of generic phishing:

- emails reference real features 
- fake pages mimic actual workflows 
- timing matches user habits 

---

## How to Disable or Limit Pendo

### 1. Block at Network Level

Block:
- *.pendo.io

### 2. Browser Blocking

- Use content blockers or policies

### 3. Citrix Settings

- Disable analytics / telemetry in admin consoles, see:

    [Citrix Web Studio - disable Pendo](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure/install-core/install-web-studio.html#optional-enable-or-disable-pendo)

    [Citrix Director - disable Pendo](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/director.html#usage-data-collection-by-pendo)

### 4. Verify

- Check DevTools (F12) → check for **pendo.io** calls

---

## Final Thought

> If data can be used to imitate behavior, it becomes intelligence.

[Richard Stallman is right!](https://www.youtube.com/watch?v=Ag1AKIl_2GM)