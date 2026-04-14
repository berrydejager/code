---
layout: post
title: When analytics becomes intelligence
date: 2026-04-13 13:37:00 +0100
description: Citrix usage tracking by Pendo.io and its hidden risks
img: header/citrix-pendo-risks.gif
tags: [citrix, pendo, analytics, social-engineering, privacy]
---

## The Hidden Layer

In many cloud and on-premises Citrix environments, analytics tooling like Pendo is quietly embedded (a.k.a. as 'opt-out feature') into the interface. Its purpose is harmless on paper:

- track feature usage 
- improve UX 
- guide users 

But in practice, it introduces a **second layer of visibility**—one that observes not just systems, but **people using them**.

---

## The token you didn’t notice

Pendo uses a structured token (often referred to as a **JZB token**) to correlate activity, see the [pendo-io JZB tool on Github](https://github.com/pendo-io/jzb)

It typically contains Base64-encoded JSON such as:

- `visitorId`
- `accountId` (which is your Active Directory domain name and account name)
- sometimes role or segmentation data

This token is:

- readable in the browser 
- accessible to scripts 
- transmitted with analytics events 

It doesn’t grant direct access—but it answers an important question:

> *Who is doing this action?*

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

- Disable analytics / telemetry in admin consoles

### 4. Verify

- Check DevTools → no pendo.io calls

---

## Final Thought

> If data can be used to imitate behavior, it becomes intelligence.
