---
layout: post
title: "LLooMA Is Connected to the Web!"
date: 2026-05-22
author: Hassan Habib
categories: [PeerLLM, Release Notes]
---

Until now, LLooMA answered every question from what it learned in training.

As of this release, LLooMA can reach beyond that horizon.

When your question needs a fact from the real world, LLooMA goes and finds it.

---

## 0/ A Quick Recap of LLooMA

For readers new to the project: LLooMA is not a model that lives on one machine.

It is a **network-native** mind.

When you send a prompt to LLooMA, an orchestrator decomposes it into smaller, independent tasks. The tasks form a directed acyclic graph of work, and that graph is dispatched across peer hosts running on volunteers’ computers all over the world.

Each host runs a local language model.  
Each task gets its own host.  
The orchestrator races and aggregates the results into a single coherent answer.

> **A single brain made of many minds. No one host sees the whole picture.**

That has been the foundation since v1.0. Today’s post is about what just changed.

---

## 1/ What "Connected to the Web" Means

Every model has a training cutoff.

Ask a static model about today’s news and it either fabricates an answer or admits it does not know.

Both outcomes are bad.

LLooMA now recognizes, per prompt, whether it needs current information from the open web. When the answer is **out there** rather than **in here**, LLooMA fetches it.

Queries that now actually work:

- *"Who is hosting the World Cup this year?"*
- *"What is the latest stable version of .NET?"*
- *"Who won the 1997 Super Bowl?"* (historical lookups count too, not just current events)
- *"What is the address of the new Qamaria coffee in Redmond?"*

And critically, queries that LLooMA correctly does **not** search for:

- *"Rewrite this email politely."*
- *"Explain how recursion works."*
- *"Thanks!"*

Search is reserved for questions that need facts.  
Generation, transformation, and small talk stay local on the peer network.

<div class="post-image">
  <img src="/assets/images/v1.2.0-llooma-web-connection.gif"
       alt="LLooMA answering a question about the latest version of C# using live web search">
</div>

---

## 2/ How LLooMA Decides

A static keyword list would be brittle and embarrassing.

Instead, LLooMA classifies every incoming prompt into one of three intents using a small language model call:

- **LOOKUP** -> the user wants a specific fact from the world. Go to the web.
- **GENERATIVE** -> the user wants content produced or transformed. The peer hosts handle it. No search needed.
- **CONVERSATIONAL** -> small talk, acknowledgements, meta-questions. Fastest path. Skip everything heavy.

The classifier also produces a clean search query (resolving *"this year"* to 2026 and *"the one in Redmond"* to the right entity from prior conversation context), a source hint (LOCATION queries lean on maps and business directories, NEWS queries hit news outlets), and a depth signal that answers one question. Does the answer live in a snippet, or do we need to follow links to find it?

None of these decisions are hard-coded heuristics.  
They are judgments.

The working rule of the architecture is simple:

> **Ideas live in the human mind. Mechanics live in code. Decisions live in the model.**

If we ever find ourselves writing `if (query.Contains("address"))` in the orchestrator, that is a sign the decision should have been made by the classifier, not by us.

---

## 3/ The Pipeline, Today

Today, LLooMA web search runs at the center.

The orchestrator owns the search call, fans the results back into a synthesis task that runs on a peer host, and returns a single coherent answer.

When search fails for any reason (bad network, rate limit, timeout), LLooMA tells you transparently instead of hallucinating.

It looks like this:

```
You -> Orchestrator
        |
        +-- classifier:    "this is a LOOKUP, search for X"
        +-- web search:    fresh facts from the open web
        +-- synthesis:     runs on a peer host -> final answer
```

This is good. But it is not yet LLooMA-native.

The search itself still lives at the center.  
Only the synthesis is distributed across the network.

This is deliberate.

Starting centralized lets us iterate fast. One set of dials. One place to tune the classifier, the providers, and the routing. Real users sending real questions, learned from in one place.

Once the behavior is proven, the same capability rolls out to host clients in a future release, and the center steps back into a fallback role.

This is exactly the path LLooMA took with compute. Centralized inference first. Then peer-first with centralized fallback once the network proved itself. The same path is coming for knowledge through JooMMA.

That changes in the next step.

---

## 4/ The Next Step: Web Search as a Distributed Tool

The original LLooMA principle: **no single host sees the whole prompt**.

The reason is privacy. A host can only leak what it sees. A host that gets one fragment of a multi-task DAG cannot reconstruct your full intent or your identity.

Web search is currently centralized only because we have not yet extended the **host capability model** to cover tools.

That is the next milestone.

Hosts will advertise the tools they can run the same way they advertise the models they host today. Web search first. Then computation. Then more.

When the rollout is complete, a single user prompt about *"the latest .NET version"* will look like this:

```
You -> Orchestrator
        |
        +-- Host A:  receives ONLY  "latest .NET stable version 2026"      (search task)
        +-- Host B:  receives ONLY  "summarize these results: <markdown>"  (synthesis task)
        +-- Host C:  receives ONLY  "stitch these outputs together"        (aggregation task)
```

Each host sees one piece.

Host A never sees who asked.  
Host B never sees the search query that produced its inputs.  
Host C sees neither.

The user identity, the question, the search, and the assembled answer live in four different places that do not talk to each other. The orchestrator is the only entity that ever holds the whole conversation, and it holds it only briefly.

> **Decentralization is not just about cost or compute. It is about cutting the surveillance surface.**

The current centralized search is the safety net.

As hosts advertise the web search capability and prove they can race against the orchestrator’s own search, the system gradually shifts the work outward. This is exactly the way inference shifted from "centralized LLM with peer fallback" to "peer-first with centralized fallback" in earlier versions of LLooMA.

Same pattern.  
New capability.

---

## 5/ Where This Goes

The web is the first non-LLM capability LLooMA can call on.

It will not be the last.

Code execution, document parsing, structured data lookups. Anything LLooMA could benefit from becomes a tool that hosts can advertise and race for.

Every new tool follows the same rule:

> **Split the work. Distribute the fragments. Let no single party see the whole.**

LLooMA started as a way to make AI cheaper and more resilient by spreading inference across volunteers.

It is becoming a way to make AI **more honest** and **more private** by spreading every kind of work the same way.

---

## Closing

The classifier, the search routing, and the transparent-failure path are in production now.

The peer-host tool distribution is queued as the next sprint of work.

If you run a host, expect a future client release that adds an opt-in `web_search` capability flag, and starts earning tokens for search tasks alongside inference.

If you are a user, try LLooMA today at <a href="https://hosts.peerllm.com" target="_blank" rel="noopener">hosts.peerllm.com</a>. Go ask it something current.

---

**Hassan**
