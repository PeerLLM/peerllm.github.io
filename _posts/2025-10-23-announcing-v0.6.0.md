---
layout: post
title: "Announcing PeerLLM Host v0.6.0"
date: 2025-10-23 18:00:00 +0000
author: Hassan Habib
categories: releases
---

This is the **second post** on the PeerLLM Blog — and I already have *a lot* to share!  

First, I want to start with a huge thank you to **@cjdutoit** and **@lactoseintolrnt** for their continued support in testing pre-releases, offering valuable ideas, and helping make PeerLLM even better with every iteration.  

Additionally, I want to thank all **63 people** who submitted to become Hosts for PeerLLM. Your curiosity, kindness, and willingness to volunteer your time, hardware, and compute are nothing short of inspiring. You’re helping this vision come to life — and that keeps me going every day.  

---

## 🚀 What is PeerLLM Host?

**PeerLLM Host** is software that runs on any PC — Windows, Linux, or macOS — allowing your machine to join the **PeerLLM Network**.  

![PeerLLM Host v0.6.0 on Mac](/assets/images/PeerLLM-v0.6.0-MacOSX.png)

Once connected, your computer becomes part of a global decentralized AI ecosystem where anyone using PeerLLM — whether through **PeerLLM Chat**, **RESTful APIs**, or our **Standard-compliant .NET library** — can leverage the compute power of your machine alongside other Hosts around the world.  

---

## 🎉 Announcing PeerLLM Host v0.6.0

This release marks the **sixth iteration** toward the first public release candidate, **v1.0.0**!  
It brings two major new capabilities and a few quality-of-life improvements.  

Let’s dive in.

---

### 🧠 0. Idle Compute Release

When a chat or API session is established with PeerLLM Network, one or more Hosts are reserved to handle that session. Tokens are distributed to balance load and provide fallback redundancy in case a Host goes offline.

With v0.6.0, we’re introducing **Idle Compute Release** — which automatically releases compute resources if a consumer has been idle for more than **15 minutes**.  

The chat window or API connection remains open with the Network, and when activity resumes, the session can continue seamlessly with a **new set of available Hosts**.  

This feature addresses a key insight from our Host testers:  
> “Memory usage kept increasing because open sessions weren’t properly released when consumers forgot to call the `/api/chats/end` endpoint.”

Now, the Network handles this gracefully, reducing unnecessary resource usage and improving overall stability for all Hosts.  

---

### 🔔 1. Update Notifications

PeerLLM Host now includes **built-in version awareness**.  
When a new version becomes available, a notification will appear at the top-right corner of the Host app interface.  

This is managed by the **Orchestrator**, which checks for updates during each Host heartbeat with the network.  

![PeerLLM on Windows Update](/assets/images/InstallUpdatePeerLLM0.6.0.gif)

I’m also working on enabling **auto-updates** — especially for those running multiple Hosts or using unattended setups (like a machine in a garage). Once approved by the user, the system will update silently and seamlessly.  

This will make it effortless to keep large-scale Host deployments current and stable.

---

### 🧩 2. What’s Next?

I’m currently developing **Semantic Kernel integration**, making PeerLLM APIs easily compatible with Microsoft’s **Semantic Kernel** framework.  

This will allow developers to connect PeerLLM alongside other AI services — switching between them effortlessly. Since Semantic Kernel is quickly becoming the standard for AI API interoperability, PeerLLM will fit right in.

Additionally. there are more enterprise-level features coming down the line such as managing a cluster of PeerLLM Hosts from one place, and so many more exciting features - stay with us!

---

## ❤️ Final Thoughts

To everyone who’s reached out with ideas, encouragement, or just good wishes — thank you.  
Your feedback means the world to me. I’m building PeerLLM for *the people*, and your voices guide its evolution.  

If you have thoughts, ideas, or feedback, please reach out anytime. Together, we’re shaping something that empowers communities, organizations, and individuals alike.  

**— Hassan Habib**  
Creator of PeerLLM  
hassan@bestbytes.ai
