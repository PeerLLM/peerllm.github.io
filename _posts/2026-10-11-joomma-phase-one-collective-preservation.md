---
layout: post
title: "Collective Preservation: JooMMA Phase One Opens PeerLLM to Storage"
date: 2026-10-11
author: Hassan Habib
categories: [PeerLLM, JooMMA, Hosts]
tags: [JooMMA, Knowledge, Storage, Hosts, The Standard, TooMMA]
description: "With PeerLLM host 3.1.0 and 3.2.0, the first phase of JooMMA is live: hosts can offer storage instead of a GPU, keep encrypted pieces of expert knowledge they cannot read, and earn for every piece they serve back when an answer needs it. The first knowledge on the network is The Standard, and answers grounded in it credit their source."
---

Most of what humanity knows has survived because somebody kept a copy. Before printing, a book lasted only as long as people were willing to copy it by hand, and a text that existed in a single place could vanish with that place. The works that survived were rarely saved by one great institution standing guard over them. They were saved because many ordinary people, in many places, each kept a copy and passed it on. Preservation has always depended less on the strength of one vault than on the number of hands holding the thing.

Knowledge is moving into a new kind of vault now. A great deal of what experts know reaches people through AI systems, and those systems keep it in very few places, owned by very few organizations. The articles, answers, books and forum posts that people wrote over decades have been gathered up to train large language models, and the people who wrote them were rarely asked, rarely credited, and almost never paid. The original may still sit on a shelf or a website, but once a model has absorbed someone's explanation, the answers it gives often carry no reliable link back to where they came from. The knowledge travels on, and the person who wrote it disappears from the record while someone else earns from what they knew.

PeerLLM began as a network of people sharing their computers to run large language models, so that AI could run on what people already have, securely, instead of on another data center. With PeerLLM host 3.1.0 and 3.2.0, both released this week, the first phase of JooMMA, the knowledge layer I [introduced in May]({% post_url 2026-05-16-joomma-introduction %}) and described in full in the [JooMMA whitepaper](/assets/files/joomma-whitepaper-v1.0.0.pdf), is live on that network. Hosts can now offer storage, keep encrypted pieces of expert knowledge on their own disks, and serve them back when an answer needs them. JooMMA is my answer to the training problem: knowledge that stays attributed to the person who wrote it, and that is built to earn for them when it is used. Hosts are paid for serving it today, and authors will be paid when the first experts join. The first knowledge on the network is my own book, [The Standard](https://hassanhabib.com/books), and answers grounded in it now credit it as their source.

PeerLLM is no longer only for people whose GPUs can run large language models: anyone with storage can join the network and take part in preserving human knowledge. You do not need the best GPU, or any GPU at all. A machine with spare disk space and a steady connection is enough to hold part of what people know and hand it back when it is needed.

## 0/ Beyond GPUs

The buildings that hold AI are part of the problem too. The answer the industry has settled on is more data centers, built everywhere, each one drawing enormous amounts of power and water to concentrate even more of the world's computing and knowledge in even fewer hands. Meanwhile, hundreds of millions of capable computers sit in homes and offices, mostly idle, with drives that are mostly empty. The resources to run and remember AI already exist. They are simply owned by everyone instead of by a few.

I call the answer *Collective Preservation*: knowledge stays alive, and stays attributed to the people who created it, when many independent hosts each hold a piece of it rather than one owner holding all of it. Collective Preservation is the reason storage now matters on PeerLLM as much as compute does. A network that can only compute can answer questions, but it cannot remember anything on its own. A network that also holds knowledge, spread across many people's machines, can keep what experts know and keep their names attached to it.

Until now, taking part in PeerLLM meant owning hardware capable of running a model at a useful speed. That is a real barrier. Plenty of people have a perfectly good computer with a modest graphics card, or none, and a few hundred gigabytes of disk they never use. Those people had little to offer a network built only around inference. Storage changes that, because the network decides where to place knowledge by asking only two questions: has the owner turned storage on, and is there room left under the limit they chose. The goal is for every drive that joins to put a little more of AI on hardware people already own, rather than behind the concrete of another data center.

Turning it on takes a minute in the host's Settings: choose how much of the drive to offer and which folder to use, and save.

<img src="/assets/images/joomma-electron-settings.png" alt="The Knowledge Storage section of the PeerLLM host Settings: a toggle to keep encrypted pieces of expert knowledge on this drive and earn for every piece served, a quota in gigabytes, the storage folder, and a status line reporting how much is stored and in how many pieces.">

The dashboard then shows what that machine is doing for the network: how much of the limit is in use, how many pieces it holds, and how much it has served today and over all time.

<img src="/assets/images/joomma-electron-dashboard.png" alt="The PeerLLM host 3.2.0 dashboard with storage on: the usual resource and estimated earnings tiles above a storage row showing storage used against the allocated limit, pieces stored, data served today, and data served all time.">

Hosts that run from a terminal get the same choice in the command-line host. `peerllm-host storage enable --quota 10GB` turns it on, `storage status` reports what is held, and the terminal dashboard gains a JooMMA Knowledge panel.

<img src="/assets/images/joomma-cli-storage-enable.png" alt="A terminal running the PeerLLM command-line host: storage status reports storage off, storage enable with a 10 GB quota turns it on, and a second storage status shows it on with the quota and folder.">

<img src="/assets/images/joomma-cli-dashboard.png" alt="The PeerLLM command-line host dashboard in a terminal, with a JooMMA Knowledge panel showing storage on, a 10 GB quota and the storage folder.">

With storage off, the host says nothing about storage at all, and the dashboard looks exactly as it did before. Taking part is a choice the owner makes, and nothing about it happens by default. If you have a drive to spare, you can [download the host and join the network](https://www.peerllm.com/join-as-host.html) today.

## 1/ Locked Boxes

The obvious question is why anyone should trust strangers with an author's work. A host is a machine owned by someone I have never met, sitting in a room I will never see. If the value of a piece of knowledge depended on the honesty of whoever stored it, Collective Preservation would be a nice idea that could never work.

Think of renting out space in your garage to hold boxes that arrive already locked, and that you have no key to. You can keep them dry, keep them safe, and hand one over when someone asks for it. You cannot open them, and you were never meant to. You know you are holding something of value, but you do not know what it is, and nothing about your job requires you to. That is the role a PeerLLM host plays for knowledge.

In practice, knowledge is divided into passages and encrypted before any of it leaves PeerLLM. The keys needed to read it are held only by BestBytes AI, and every passage is checked each time a host returns it, so a host that returns anything other than what it was given simply stops being used to store or serve it. The trust does not rest on what strangers promise. It rests on what the system lets them do.

I also want to be honest about where control still sits. BestBytes AI holds the keys, and PeerLLM keeps a central copy that serves a passage whenever no host can. In this first phase the network spreads out who holds knowledge, not who can unlock it, and hosts could not yet keep it readable on their own. Distributed storage is the first step toward Collective Preservation, not the whole of it, and moving more of that independence to the network is part of the work ahead.

Beyond the code, I wanted this written down. The [PeerLLM Host Agreement](https://www.peerllm.com/host-agreement.html), updated this week and now at version 2.3.0, says what storing knowledge means for a host: it may hold encrypted passages, and it may not attempt to decrypt, inspect, copy, or alter them. The [Privacy Policy](https://www.peerllm.com/privacy.html) describes the same arrangement for everyone who uses the network. A promise that lives only in code is easy to forget, and a promise that lives only in a document is easy to break, so I wanted both.

## 2/ My Own Book

The first knowledge I put on the network had to be something I could give away without asking anyone's permission. The Standard is the closest thing I have to that: years of thinking about how software should be built, written down, and already shared freely with anyone who wants it. All sixteen of its chapters are now on PeerLLM, divided into passages, encrypted, and held by hosts who turned storage on.

When a question touches what The Standard covers, the network finds the relevant passages, grounds the answer in them, and credits The Standard as the source. That credit matters more than it might seem. An answer that says where it came from can be checked. If it is wrong, you know exactly where to look, and if it is right, you know whom to thank. An AI system that answers from nowhere asks you to trust it, while one that answers from somewhere invites you to verify it.

Here is what that looks like in practice. Asked whether it is fine to put some business logic in a broker, LLooMA2.0 answers from the passages of The Standard it was given, quotes the chapter, and ends by naming its source.

<img src="/assets/images/joomma-grounded-answer.png" alt="A PeerLLM chat with LLooMA2.0 selected as the model: asked whether it is ok to add some business logic to a broker, LLooMA2.0 answers no, quotes The Standard on why business logic is outside a broker's purpose, explains that brokers wrap external resources so business logic stays pluggable, and ends with the line Source: The Standard: 1. Brokers.">

Attribution is half of the point. The other half is that people whose knowledge is used should be paid for it, and that is the part this first phase exists to make possible.

## 3/ Honest Accounting

The hardest problem in JooMMA has never been storing knowledge or encrypting it. It is distribution and compensation: making sure that what is placed in front of the model is exactly what was served, and that what was served is what gets compensated, fairly, between the author, the host, and the network. Every other part of the design rests on getting that chain right.

So every time a passage is retrieved and placed in front of the model to ground an answer, the network records which piece of knowledge and which version was read, and whether a host's copy or PeerLLM's central copy served it. The record holds identifiers and counts, and it does not hold the question, the answer, or the passage text. It measures what was served into an answer, not how much a passage shaped it, and payment follows what was served.

When an author updates their knowledge, the new version replaces the old one, and hosts retire the old pieces, so the network serves what the author currently stands behind rather than something they have since corrected. The earlier versions are not thrown away: PeerLLM keeps them centrally so they can be restored, and when an author deletes their work, the keys to every version are destroyed, so the pieces left on any host, even one that was offline at the time, can no longer be read.

I think of it the way a careful publisher thinks of royalties. A royalty statement that cannot be traced back to the copies actually sold is not an account, it is an estimate, and authors learn very quickly to distrust estimates. That is why the record came first, and it is also why paying hosts turned out to be straightforward. Every passage a host serves now counts toward its earnings at the same per-token rate as the tokens it processes for language models, and that includes every passage served since storage first went live. Authors will be compensated on top of the same accounting when the experts arrive, so that every payment can point to the work that earned it.

This is the very first release of JooMMA, and there is much more to come.

## 4/ The Experts

What comes next is people. I will begin a slow rollout of an SME portal in PeerLLM, where a group of subject matter experts, invited from all walks of life, can share their knowledge and their experience with the world. A mechanic who has spent thirty years listening to engines knows things no manual says. So does a nurse who has worked night shifts for decades, or a teacher who has found a way to explain fractions that finally lands. That kind of knowledge rarely gets written down, and when it is, the person behind it is usually forgotten.

The portal is built around three things: the expert's name stays attached to their knowledge, the knowledge itself is preserved across the network, and the expert is compensated when what they shared is used to answer someone's question. I am starting slowly on purpose. Trust in a knowledge network is earned one expert and one passage at a time, and I would rather grow it carefully than fill it quickly.

## 5/ Shared Tools

In December I wrote that intelligence has three parts: data, decision, and direction. LLooMA covers decision, by routing the work of running models across the network. JooMMA now begins to cover data, by holding knowledge and keeping its authors visible. The next major release covers direction, and it is called TooMMA.

TooMMA is tool sharing on hosts. Someone might want to reach the web through a host, run code in a sandboxed, isolated environment, or use any other network-approved tool that a host chooses to offer. In the same way that a host can lend its GPU to run a model, or its disk to hold knowledge, it will be able to offer abilities that let AI do things rather than only say things. It is coming soon, and I will write about it properly when it arrives.

## 6/ The Future

This release is for hosts who have storage to share and want a reason to share it. It is for people who want an AI network that honors the knowledge it uses and credits the people who created it. More than either of those, it is for anyone who wants a flavor of AI that puts people first: one where contributing does not require the most expensive hardware, and where the person who knew something does not vanish the moment a machine learns it. If that is the AI you want, [join PeerLLM today](https://www.peerllm.com/join-as-host.html).

The question I keep returning to is not how much a network can compute. It is how much of what people know it can keep, and whether it can keep their names with it. The copyists who saved the books still read today did not need to own a library. They needed a quiet place, a steady hand, and the belief that what they were holding was worth passing on. A spare disk and a host that is turned on are not so different, and I hope a great many people decide that what the network is holding is worth keeping too.

---

**Hassan**
