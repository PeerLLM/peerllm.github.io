---
layout: post
title: "PeerLLM v0.10.0 — Faster Dashboard, Logs, Compatibility Intelligence, GPU Fixes, and Linux Packaging"
date: 2025-12-07
author: Hassan Habib
categories: releases peerllm
version: 0.10.0
---

PeerLLM **v0.10.0** is now available — and this release brings some of the most impactful usability and reliability upgrades to the Host application so far. From a dramatically improved dashboard to compatibility intelligence, real-time GPU monitoring, and full logging support, this update elevates the entire PeerLLM hosting experience.

Below are the highlights of what's new, what's improved, and what it means for PeerLLM hosts worldwide.

---

## 0. Major Dashboard UX Enhancements

The dashboard received a **major visual and functional overhaul** designed to improve clarity, performance, and real-time observability.

<p align="center">
  <img src="/assets/images/dashboard-new-ui-v0.10.0.gif" width="80%">
</p>

### New & Improved:
- **Incoming vs. Outgoing token telemetry**
- **A new speedometer-style gauge** showing response speed
- **Network peer indicators** (active peers vs total peers)
- Cleaner, more intuitive layout
- Smoother interactions and real-time updates

This is the most polished and informative PeerLLM dashboard to date — and the foundation for more monitoring features coming soon.

---

## 1. New Logs Section — Debugging Made Simple

PeerLLM now includes a **dedicated Logs section**, allowing you to browse logs **by date** directly inside the Host app.

<p align="center">
  <img src="/assets/images/logs.png" width="80%">
</p>

This is extremely helpful for:

- Investigating issues on your own host
- Sharing clear diagnostics with PeerLLM support
- Understanding what your host was doing at any point in time

Troubleshooting is now more transparent, faster, and easier than ever.

---

## 2. Intelligent LLM Compatibility Detection

A major new capability in v0.10.0: the **LLM compatibility indicator**.

PeerLLM now evaluates whether your host can realistically support a selected model *before* you download it.

<p align="center">
  <img src="/assets/images/llm-compatibility.png" width="600">
</p>

This prevents situations where:

- A large model is fully downloaded
- …only to discover it cannot load or run on your machine

This saves time, bandwidth, storage, and frustration — across both **approved** and **remote** LLMs.

---

## 3. Fixed GPU Monitoring on Windows & Linux

GPU monitoring has been fully repaired and now displays accurate utilization in real time.

<p align="center">
  <img src="/assets/images/gpu-monitoring.png" width="300">
</p>

Previously, GPU usage always showed **0%** regardless of workload.  
Now:

- GPU utilization is accurate
- Windows and Linux Hosts are fully supported
- Resource dashboards finally reflect reality

This dramatically improves performance diagnostics and model tuning.

---

## 4. LLM Test Timeout (60s Safety Mechanism)

To prevent frozen or stalled hosts, LLM tests now have a **60-second timeout**.

If the test exceeds this duration:

- It is automatically terminated
- The host is marked **incompatible** with that model

This ensures stability, avoids runaway tests, and creates predictable behavior.

---

## 5. Faster, Instant Startup Experience

PeerLLM Hosts now start up **significantly faster**.

- Stats load instantly
- Host state initializes immediately
- Dashboard becomes usable right away

Startup friction is gone — the app feels smooth, light, and responsive.

---

## 6. New Linux Packaging — Debian & Arch

PeerLLM now ships two official Linux builds:

- **Debian-based distros (.deb)**
- **Arch-based distros (.pacman)**

This dramatically expands compatibility and makes installation seamless across the most popular Linux environments.

---

# Introducing PeerLLM Hosts Portal

Alongside v0.10.0, we’re also excited to highlight the **PeerLLM Hosts Portal**, a centralized web interface where hosts can monitor their systems and manage their accounts.

👉 **https://hosts.peerllm.com**

With the Portal, you can:

### 0. View Live Network Stats
See total peers, active peers, compute availability, and more.

<p align="center">
  <img src="/assets/images/host-portal-dashboard.png" width="70%">
</p>

### 1. Monitor All Your Hosts
Track every Host you run across devices and platforms.

<p align="center">
  <img src="/assets/images/host-portal-hosts-monitoring.png" width="70%">
</p>

### 2. Access Software Downloads
Always get the latest stable builds for Windows, macOS, and Linux.

<p align="center">
  <img src="/assets/images/host-portal-downloads.png" width="70%">
</p>

### 3. Manage Your Account
Reset your password, manage sessions, and control account settings.

The Portal is the new home for PeerLLM operators and will continue to grow with every release.

---

# 🚀 Final Thoughts

PeerLLM v0.10.0 focuses on **speed, intelligence, clarity, and stability** — making hosting easier, safer, and more predictable than ever before.

As always, thank you to every host, contributor, and early adopter helping build the decentralized intelligence network of the future. Your feedback directly shapes every release.

More improvements are on the way — stay tuned for v0.11.x!
