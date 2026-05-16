---
layout: post
title: "Introducing JooMMA: The Knowledge Layer for PeerLLM"
date: 2026-05-16
categories: peerllm joomma decentralized-ai knowledge
---

Artificial intelligence is often described as a compute problem. People talk about bigger models, faster GPUs, lower latency, better infrastructure, and cheaper inference. All of these things matter, but they are only one side of the story. Intelligence does not come from compute alone. Intelligence also depends on knowledge, and knowledge usually comes from people.

That is where **JooMMA** comes in. JooMMA is the knowledge layer of PeerLLM. While **LLooMA** focuses on the compute side of PeerLLM by helping route AI inference across a decentralized network of hosts, JooMMA focuses on the knowledge side. It is designed to help PeerLLM understand where useful knowledge comes from, who contributed it, how it was validated, and how value should flow back to the people and hosts who made that knowledge available.

The idea is simple. When useful human knowledge helps an AI system answer a question, the person who contributed that knowledge should not disappear from the process. They should be credited, their contribution should be traceable, and when that knowledge is used, they should have a path to be paid.

## The Problem: Knowledge Gets Erased

Today’s large language models are trained on massive collections of text. They learn from books, articles, websites, documentation, code, forums, research papers, and many other sources of human knowledge. This is part of what makes them useful. They can answer questions because somewhere, at some point, people wrote, explained, taught, argued, documented, discovered, or shared something useful.

The problem is that once this knowledge is absorbed into a model, the original source often becomes invisible. The model may produce a helpful answer, but it usually cannot clearly say which human contribution made that answer possible. It cannot reliably point back to the exact person, explanation, correction, or expertise behind the answer. The knowledge becomes compressed into weights, and the human source is erased from the economic flow.

That creates a serious problem. If we cannot see where knowledge came from, it becomes very hard to pay people for it. It becomes hard to correct it. It becomes hard to verify it. It becomes hard to know whether the answer came from a trusted expert, an outdated source, a random post, or something the model simply guessed. The more powerful AI becomes, the more important this problem becomes.

PeerLLM is built on a different belief. Useful work should be visible, traceable, and rewarded. If someone contributes compute, the network should know who performed that work. If someone contributes knowledge, the network should also know who supplied that knowledge. A fair AI economy cannot only reward the companies that own the biggest models. It should also reward the people whose knowledge makes those models useful.

## What JooMMA Does

JooMMA does not try to become another giant training dataset. It is not designed to scrape the internet, collect everything in advance, and compress it into a model. Instead, JooMMA works at inference time, which means it becomes active when a user actually asks a question.

When PeerLLM receives a request, JooMMA can check whether there is already verified knowledge that can help answer that request. If verified knowledge exists, JooMMA retrieves it and gives it to the model as grounded context. This helps the model answer with better support, clearer attribution, and more accountability.

If verified knowledge does not exist, that missing information becomes a knowledge gap. A knowledge gap means the network has discovered something users are asking for, but the system does not yet have a strong verified shard of knowledge to support the answer. Instead of pretending that the gap does not exist, JooMMA makes the gap visible.

That gap can then be routed to a verified contributor in the right domain. For example, a medical question should not be routed to a software engineer unless the question is actually about medical software. A legal question should not be routed to someone just because they are generally smart. JooMMA is designed around domain-scoped expertise, which means contributors are verified for specific areas of knowledge.

Once a contributor answers the question, the answer goes through validation. The system can use automated checks to look for obvious problems, contradictions, overlap with existing content, or weak claims. Then human review can be used to evaluate the answer in context. Once the answer is approved, it becomes a reusable unit of knowledge called a **semantic shard**.

A semantic shard is more than a paragraph of text. It is a piece of knowledge with history around it. It can include who authored it, what domain it belongs to, when it was created, how it was reviewed, whether anyone disagreed with it, how often it was retrieved, and which hosts are making it available. In simple terms, a semantic shard is knowledge with provenance.

## How JooMMA Works with PeerLLM

PeerLLM has two major layers that can work together. **LLooMA** handles compute. **JooMMA** handles knowledge. LLooMA asks where the AI work should run. JooMMA asks what trusted knowledge should be used to ground the answer.

When a user sends a request to PeerLLM, the system can route the compute through LLooMA. That means the model execution can happen through the decentralized PeerLLM host network instead of depending only on one centralized provider. This is the compute side of the system.

Before or during that generation process, JooMMA can search for verified shards that are relevant to the user’s question. If it finds them, those shards can be inserted into the model’s context. The model can then answer with support from knowledge that has attribution, validation, and usage tracking attached to it.

This is important because the user still asks one question and receives one answer. The complexity stays inside the PeerLLM network. Behind the scenes, LLooMA helps decide how the request should be executed, and JooMMA helps decide what knowledge should support the response. The result is a system where both execution and knowledge can be coordinated, measured, and rewarded.

LLooMA can exist without JooMMA by relying on model weights alone. JooMMA can also exist as a knowledge archive without LLooMA. But together, they create something much more powerful. LLooMA decentralizes inference, and JooMMA decentralizes grounded knowledge. One moves the work of running AI into the network. The other moves the value of human knowledge back into the network.

## Why This Matters

The current AI economy mostly rewards the organizations that control models, infrastructure, platforms, and distribution. Those organizations can collect data, train models, sell access, and capture most of the economic value. Meanwhile, many of the people whose knowledge made the system useful receive no attribution and no ongoing participation.

JooMMA is designed to challenge that pattern. It gives PeerLLM a way to treat knowledge as a live contribution instead of a one-time extraction. A domain expert, researcher, educator, technical writer, engineer, lawyer, doctor, mechanic, historian, or experienced practitioner should be able to contribute useful knowledge and have that contribution remain visible when it is used.

This does not mean every piece of text deserves payment forever. It means that verified knowledge used to ground real answers should have an economic path back to the person who contributed it. If an expert writes a useful explanation and that explanation helps answer future questions, the system should be able to record that usage and reward the contributor.

This changes the relationship between AI and human expertise. The expert is no longer treated as invisible raw material. Their knowledge becomes part of an active knowledge layer. It can be retrieved, cited, reviewed, challenged, improved, hosted, and paid for over time.

## Verified Knowledge, Not Random Content

JooMMA is not meant to be a free-for-all content platform. The goal is not to collect as much text as possible. The goal is to build a trusted knowledge layer that can support AI answers with higher confidence.

That requires contributor verification. A contributor should be verified for the domain they are contributing to. Someone may be very strong in C# and software architecture, but that does not automatically make them qualified to contribute medical guidance. Someone may be an excellent cardiologist, but that does not automatically make them qualified to answer questions about constitutional law.

This domain-scoped verification matters because trust is not general. Trust depends on context. In real life, we already understand this. We do not ask a dentist to certify a bridge design, and we do not ask a civil engineer to diagnose a heart condition. JooMMA brings that same basic idea into the knowledge layer of AI.

Submissions also need validation. Automated checks can help catch simple problems quickly, but human expert review remains important, especially in high-stakes domains. The point is not to replace human judgment with automation. The point is to combine automation and human review so that knowledge can be added carefully and responsibly.

## Payment by Use

JooMMA’s economic model is based on retrieval. When a semantic shard is retrieved and inserted into the model’s context, the system records that usage. This makes payment easier to understand and easier to audit.

The system does not need to solve the impossible philosophical problem of proving exactly which sentence in the final answer came from which source. Instead, it uses a simpler rule. If a shard was retrieved and inserted into context to help ground the answer, then that shard participated in the answer. That retrieval event can be recorded, and the contributor can be paid according to the network’s rules.

In the Phase 1 design, the author receives the larger share of the shard payment, while the hosts who store and serve the shard receive the remaining share. This is because verified knowledge is the scarce contribution. Hosting is important, but the original knowledge is the core value. The person who created the verified shard should receive the majority of the benefit when that shard is used.

The contributor also does not need to be online at the exact moment their knowledge is used. If a fallback host serves the shard while the original contributor is offline, the author can still receive the author share. This is similar to how other knowledge industries work. An author does not need to personally operate a bookstore every time a book is sold. The value comes from authorship, not from being online at the moment of delivery.

## Hosting Knowledge in the Network

JooMMA also creates a role for hosts. A contributor may be the primary host of their own shards, but fallback hosts can also store and serve shards to improve availability. This helps make the network more resilient. If the original contributor is offline, the knowledge does not have to disappear from the system.

This fits naturally with PeerLLM’s broader mission. PeerLLM already allows people to contribute compute. JooMMA extends that idea to knowledge availability. Hosts can help serve verified shards, and when those shards are retrieved, the host pool can receive a portion of the payment.

This creates a network where people can participate in more than one way. Some people may contribute GPUs and run models. Some people may contribute verified knowledge. Some may help host and serve knowledge shards. Over time, PeerLLM can become not just a decentralized inference network, but a decentralized intelligence economy.

## Handling Disagreement

Real knowledge is not always clean. Experts disagree. Fields change. Research evolves. Some questions have more than one responsible answer. In many AI systems, disagreement gets flattened into a single confident response, even when the underlying topic is contested.

JooMMA takes a different approach. It treats disagreement as part of provenance. If a reviewer disagrees with a submitted shard, that disagreement does not always have to destroy the shard. In some cases, the disagreement itself should be preserved. The system can attach a dissent record that explains the objection, the reviewer’s domain, and the limits of the claim.

This is especially important in high-stakes or evolving domains. A user may need to know that there are multiple expert views. Instead of pretending that one answer is final, PeerLLM can surface the disagreement in a grounded way. This makes the system more honest and gives users a clearer view of where knowledge is settled and where it is still being debated.

The goal is not to make AI less useful. The goal is to make it more truthful about uncertainty. A system that admits disagreement when disagreement exists is more trustworthy than a system that hides complexity behind a confident answer.
## Coming to PeerLLM Soon

Over the next few sprints, I will start rolling JooMMA into the PeerLLM network.

The first version will be practical and focused. The goal is not to launch every part of the full vision at once. The goal is to begin connecting real user demand with real knowledge gaps, then build the foundation for contributors, validation, retrieval, attribution, and payout tracking over time.

This rollout will happen gradually. PeerLLM already has the foundation for decentralized compute through hosts and LLooMA. JooMMA adds the next layer by helping the network understand what knowledge is being used, where that knowledge came from, and how contributors should participate in the value they create.

As this work moves forward, the PeerLLM network will become more than a place where people can contribute compute. It will start becoming a place where people can contribute expertise.

## The Bigger Vision

JooMMA is part of the bigger PeerLLM vision. Decentralized AI should not only decentralize compute. It should also decentralize knowledge, attribution, validation, hosting, and economic participation.

LLooMA asks who performs the inference work. JooMMA asks who supplied the knowledge that made the answer useful. Both questions matter. If we only decentralize compute, we still leave knowledge trapped in centralized systems. If we only build knowledge archives without execution, we do not create a full AI network. PeerLLM needs both layers working together.

This is how we move from AI systems that extract from people to AI systems that include people. The future of AI should not be a world where human knowledge is scraped, compressed, and sold back to humanity with no memory of who contributed it. The future should be a network where people can contribute compute, knowledge, tools, and expertise, and where the value created by those contributions can flow back to them.

JooMMA is a step toward that future. It is a provenance-preserving knowledge layer for PeerLLM, designed to keep human expertise visible, trusted, and rewarded.

Read the full whitepaper for JooMMA <a href="/assets/files/joomma-whitepaper-v1.0.0.pdf" target="_blank">here</a>