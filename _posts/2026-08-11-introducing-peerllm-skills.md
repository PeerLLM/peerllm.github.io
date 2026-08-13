---
layout: post
title: "Introducing PeerLLM SKILLs: A Portal to Author, Verify, and Publish Agent Skills"
date: 2026-08-11
author: Hassan Habib
categories: [PeerLLM, Skills, AI]
tags: [AgentSkills, Evaluation, CrossModel, Registry, RFC2119]
description: "A skill can work perfectly on one model and quietly fall apart on another — and until now, nobody checked. PeerLLM SKILLs is the portal where you author a skill as typed, RFC-2119 blocks, verify it across real LLMs on the PeerLLM network, and publish only the ones that pass — each carrying a Skill Score as evidence."
image: /assets/images/skills-hero.png
---

An **agent skill** is a small, portable instruction set — a `SKILL.md` folder that teaches an agent how to do one thing well: write a proper commit message, enforce an architecture, gate a dangerous action, synthesize a research brief. It loads when the model decides it's relevant, and from that point on it steers behavior.

Skills have quietly become one of the most important units of AI work. They're how you get a general model to behave like a specialist, without fine-tuning, without a new deployment, without touching weights. You write the instructions once, and every capable agent can pick them up.

But there's a problem hiding in that sentence — **"every capable agent."**

A skill that works beautifully on one model can silently degrade on another. The same `SKILL.md` that produces flawless output on a frontier model might never even trigger on a 7B, or trigger and then ignore half its own rules. You wouldn't know. There's no test suite for a skill, no CI, no report card. You write it, it looks right, you ship it — and its actual behavior across the models people run is a black box.

**PeerLLM SKILLs** is the portal that closes that box. It's where you **author** a skill, **verify** it against real LLMs on the PeerLLM network, and **publish** the ones that pass — each carrying a **Skill Score** that says, with evidence, exactly which models it works on and how well.

<div class="post-image">
  <img src="/assets/images/skills-hero.png" alt="The PeerLLM SKILLs landing page: 'Build agent skills that work on every model,' above a live PeerLLM network panel showing hosts serving, models live, requests per hour, and tokens served.">
</div>

## 0/ What PeerLLM SKILLs Is

PeerLLM SKILLs is three things wrapped around one idea.

The idea: **a skill isn't done when it reads well — it's done when it's been proven to work across the models it will actually run on.**

The three things:

- **Author** — an in-browser editor where you write a skill as *typed blocks* instead of freeform prose. Each block has a job, and the editor compiles them into a clean, correct `SKILL.md`.
- **Verify** — a cross-model evaluation engine. Point it at a set of models, and it fans the skill out across all of them on the PeerLLM network, then a judge model grades the results on three dimensions and hands back a **report card**.
- **Publish** — a registry. The skills that earn a good score get published — public or private, versioned, installable in one click or one command — and every listing wears its evidence.

Think of it as **source control, CI, and a package registry for skills** — except the "CI" isn't a linter, it's real models running your skill and being graded on whether they actually followed it.

## 1/ The Problem: A Skill That Works on One Model Isn't a Skill That Works

Here's the failure mode that motivates the whole product.

You write a skill. You test it in the one place you happen to be working — say, a strong frontier model in your IDE. It's perfect. You publish it. Someone else picks it up and runs it on a smaller local model, or a different vendor's model, or the exact same model six weeks later after it's been updated. And it just... doesn't fire. Or it fires and produces something subtly wrong. Or it follows four of your six rules and drops the two that mattered most.

None of that shows up when you *read* the skill. `SKILL.md` is prose, and prose looks equally convincing whether or not any given model will obey it. The gap between "this is well-written" and "this actually changes behavior on model X" is invisible until someone hits it in production.

So the honest question for any skill is not "is this well-written?" It's **three** questions, per model:

1. **Triggering** — from its `description` alone, does the model even decide to invoke the skill in the situations it's meant for?
2. **Instruction-following** — once loaded, does the model actually do what the `SKILL.md` says?
3. **Outcome correctness** — did the end result match what was expected?

A skill can pass one and fail the next. It can trigger reliably and then ignore its own rules. It can follow every rule and still produce the wrong answer because the rules were incomplete. **Three independent things can go wrong, and they go wrong differently on different models.** The only way to know is to run it — a lot, across many models — and grade it.

That's exactly what PeerLLM SKILLs does, and it's the wedge the whole portal is built around.

## 2/ Author — Typed Blocks, Not a Wall of Prose

Most skills are written the way you'd write a note to a colleague: a paragraph of context, a bulleted list of "do this, don't do that," maybe an example. It reads fine to a human. But models don't respond equally to every phrasing, and a wall of undifferentiated "musts" is one of the fastest ways to get a skill ignored.

PeerLLM SKILLs replaces freeform prose with **typed blocks**. Each block has one job, and the editor compiles them into a clean `SKILL.md`:

- **Overview** — one or two sentences on what the skill does and the outcome it produces. The anchor at the top.
- **When to use** — the activation guidance. This, plus the description, is what *triggering* keys off.
- **Step** — one action in an ordered procedure. Compiles to a numbered list.
- **Example** — a worked input → output. Models imitate examples strongly, so this pins down the exact shape you want.
- **Rules (RFC 2119)** — the constraints, ranked by how binding they are.
- **Tool rules** — routing policy: "when this situation arises, call that tool."
- **Notes, Warnings, References** — supporting context that helps the agent get it right without being a hard rule.

The heart of it is that fifth category. The rule blocks use **RFC 2119** keywords — `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, `MAY` — and they are *not* interchangeable. Each one says something precise about how binding a rule is, and the single most useful question when writing one is: **"what happens if the agent ignores this?"**

- If ignoring it breaks correctness, safety, or a required format → **MUST / MUST NOT**. Non-negotiable. Use sparingly; every MUST is a promise the model has to keep.
- If it's the right default a capable agent could justifiably override in a specific case → **SHOULD / SHOULD NOT**. Best for style, quality, and preferences.
- If it's genuinely optional, permitted but not pushed → **MAY**.

This isn't pedantry. **A wall of MUSTs is brittle — pile up enough absolute rules and models start ignoring all of them.** Ranking your constraints by how binding they actually are is one of the highest-leverage things you can do to make a skill hold up across models. The typed editor makes that ranking a first-class act instead of a matter of prose tone.

And it stays portable. A skill imports and exports as a single `SKILL.md` file. Any model — inside PeerLLM or out — can generate one, and pasting it into the editor's **Import** button breaks it back into blocks. The typed structure is a better way to *work*; the artifact you ship is still just a plain, standard skill folder that any agent runtime understands.

<div class="post-image">
  <img src="/assets/images/skills-guide.png" alt="The Author's Guide: choosing the right RFC-2119 requirement level (MUST, SHOULD, MAY) with a quick decision guide, and the typed block reference showing Overview and When-to-use blocks compiling to SKILL.md.">
</div>

## 3/ Verify — The Report Card

This is the part nothing else does, and it's the reason the portal exists.

Once you've authored a skill, you pick a set of candidate models and run an evaluation. The engine generates a test suite for the skill and fans it out across every model you selected — each running the skill on real prompts. Then a **strong anchor model acts as the judge**, grading each run on the three dimensions from Section 1: did it trigger, did it follow the instructions, was the outcome correct.

What comes back is a **report card** — a matrix of *skill × model*, with a grade in each cell and the judge's rationale behind it. Out of that matrix the portal computes a single headline number: the **PeerLLM Skill Score**, out of 100, with a letter grade and a confidence level that reflects how many runs and models are behind it. The score isn't a black box either — it's a stated formula, `0.8 · capability + 0.2 · consistency`: mostly *how well the skill does across models*, partly *how evenly it does it*, so a skill that's brilliant on one model and broken on the rest can't hide behind its best result.

The numbers are real, and they're often humbling. Here are actual scores from the portal right now:

- **the-standard-ethical-skill** — 98/100 (A), evaluated across 3 models. Holds up almost everywhere.
- **the-standard-architecture** — 100/100 (A) on LLooMA 2.0, but only 1 run so far — low confidence, more evaluation needed before you'd trust it broadly.
- **the-standard-versioning** — 70/100 (D). Scores 100 on GPT-OSS-20B and 50 on Qwen3-14B. *The same skill, a 50-point spread across models.*
- **a biopharma research-brief skill** — 41/100 (F) across 5 models. Reads reasonable; doesn't hold up under grading.

That third example is the entire thesis in one line. **The same skill, unchanged, swings 50 points depending on the model.** Without cross-model evaluation you'd have shipped it, and half your users would have gotten the good version and half the broken one, and you'd never have known which was which.

The report card also surfaces **Skill Lift** — how many points the skill moved a model on task correctness *versus running that model with no skill at all*. Because a skill that scores well but didn't actually change anything isn't a skill, it's decoration. Lift separates the skills that carry their weight from the ones that just happen to sit next to a capable model.

<div class="post-image">
  <img src="/assets/images/skills-matrix.png" alt="The Skill × LLM matrix: every skill as a row, every model as a column, a letter grade in each cell. the-standard-versioning shows 100/A on GPT-OSS and 50.5/F on Qwen3-14B — the same skill, a 50-point spread across models.">
</div>

## 4/ The Evidence Goes Deeper Than a Grade

A single score would be a nicer bumper sticker than what most skills have today, but it's not where the portal stops. Open any skill and the grade unfolds into the evidence behind it — the part that makes it a report card rather than a rating.

- **A per-model evidence table.** For every LLM the skill ran on, you get its score, its grade, its *instruction-following* and *outcome-correctness* broken out separately, its lift, how many runs stand behind it, its confidence, and its trend since last time. When a skill scores badly you can see *which* dimension gave out — did it stop triggering, or trigger and then ignore the rules?
- **Skill health.** A set of bars — compatibility, cross-model coverage, evaluation confidence, regression stability, freshness, security — that turn "70/D" into a diagnosis instead of a verdict. A skill can be strong on freshness and security and still be dragged down by thin coverage or low confidence, and health shows you exactly that.
- **Continuous evaluation.** This is the one I'd point to first. A skill can declare **regression gates** — *average score drop ≤ 5 points, no Tier-1 model below a B, no model regressed more than 5 points, instruction adherence ≥ 85* — and every new version is checked against them, passing or failing as a unit. That's **CI for agent behavior**: the same discipline that stops a bad commit from merging, pointed at stopping a skill edit that quietly breaks the skill on half its models.
- **A security scan.** Every skill's files are statically scanned for prompt injection, credential exfiltration, destructive commands, hidden text, and suspicious encoding. Critical or high findings block the *PeerLLM Verified* mark. A skill is code that steers an agent, and it gets read like code before it earns a badge.

And crucially, the evidence isn't the author's to dictate. Every skill page has a **"challenge this score"** path: anyone can run their own independent evaluation — their own models, their own nodes, their own test set — and those runs join the evidence graph. **Reproduced results raise a skill's confidence; divergent ones surface exactly where it doesn't hold.** No author owns the truth about their own skill; the network does, adversarially, out in the open.

## 5/ Publish — A Registry Where Every Listing Wears Its Evidence

A verified skill is only useful if people can find it and use it. So the third pillar is a registry.

Publishing gives a skill a home: an owner handle and name (`hassanhabib/the-standard-architecture`), a version, and a visibility — **public** for the world or **private** for you and your team. From there anyone can:

- **Browse and search** — not just by name, but by evidence. *"governance skills that score A on Qwen." "coding skills tested on 5+ models." "verified skills with a lift above 20."* You search the skills by how they actually performed, not by how their READMEs read.
- **Install in one step** — download the skill folder as a `.zip`, or pull it from the command line with the `peerllm-skills-cli` (`npm i -g peerllm-skills-cli`, then `skills pull <handle>/<name>`). There's a REST API and a client library too, so skills flow into whatever pipeline you already run.
- **See the evidence up front** — every listing carries its Skill Score, its per-model grades, its confidence, its lift, and when it was last evaluated. The badge isn't a vanity sticker; it's the report card, attached to the artifact, everywhere the artifact goes.
- **Embed a live badge** — copy a one-line Markdown badge into any README, and it stays live: as new evaluations land, the badge updates itself. The evidence follows the skill even into repos that have nothing to do with PeerLLM.

That third point is what makes the registry different from a folder of `SKILL.md` files on the internet. **The evidence travels with the skill.** When you pick one up, you're not trusting a description — you're reading a grade earned by real models, and you can click straight through to the runs behind it.

<div class="post-image">
  <img src="/assets/images/skills-registry.png" alt="A published skill's evidence page for hassanhabib/the-standard-versioning: PeerLLM Skill Score of 70/D with the capability-plus-consistency formula, a per-model evidence table, skill-health bars, and continuous-evaluation regression gates.">
</div>

## 6/ Why It Runs on the PeerLLM Network

Cross-model evaluation has an obvious cost problem. Grading one skill properly means running it dozens of times across many models — and if every one of those runs is a metered call to an external provider, "verify across every model" quietly becomes "verify across the two models you can afford."

This is where the rest of PeerLLM does its job. The eval engine fans its runs out across the **decentralized PeerLLM network** — the same pool of community-owned machines that powers everything else we've built — through the orchestrator's OpenAI-compatible `/v1` endpoint. Grading a skill across many models draws on network compute instead of running up a per-token bill with a handful of vendors.

That single fact changes what's economically reasonable. Testing a skill on *one* model is cheap enough that it happens by accident. Testing it on *ten* — which is the only test that actually tells you whether the skill is portable — is only reasonable when the compute underneath is a shared network rather than a stack of metered API keys. **The verification that matters most is exactly the verification that's too expensive to do the centralized way.** PeerLLM SKILLs is what that network makes affordable.

As I write this, the network behind it is live: hosts serving, models available, requests flowing, tens of millions of tokens served — the same infrastructure, now pointed at making skills trustworthy.

## 7/ Why This Matters

Skills are becoming a real software artifact — something you write, share, depend on, and build products around. And every real software artifact eventually grows the same three things around it: **a way to author it well, a way to prove it works, and a way to distribute it.** Code has editors, tests, and package registries. Skills, until now, had a text file and good intentions.

The missing piece was never authoring — people have been writing skills for a while. The missing piece was **verification**. Without it, a skill is a claim. "This enforces our architecture." "This gates dangerous actions." "This produces a decision-grade brief." Says who? On which model? Graded how? A skill with no evidence behind it is a promise you're asked to take on faith, and the numbers above show how often that faith is misplaced — a 50-point spread on the same skill, an F on something that reads perfectly fine.

PeerLLM SKILLs turns the claim into evidence. Author it as typed blocks so it's structured to be followed. Verify it across real models so you know where it holds and where it breaks. Publish it with the report card attached so the next person doesn't have to take it on faith either. **The skill stops being a promise and becomes a proven, portable, evidence-backed unit of work.**

That's the shift: from *"here's a skill, trust me"* to *"here's a skill, and here's exactly how it scored on every model you care about."* Once you've seen the difference between those two, it's hard to go back to shipping skills blind.

## 8/ Try It

PeerLLM SKILLs is live and experimental at **[skills.peerllm.com](https://skills.peerllm.com)**. If you write skills — or depend on ones other people wrote — it's worth an afternoon:

- **Author** a skill from typed blocks, or import a `SKILL.md` you already have and watch it break apart into structure.
- **Verify** it across a handful of models and read the report card. Be ready to be surprised by where it fails.
- **Publish** the ones that earn it, and pull others' skills straight into your workflow with the CLI.

A skill you haven't tested across models isn't a skill that works — it's a skill that worked *once, somewhere, on something.* PeerLLM SKILLs is how you find out which one you actually have.

---

And it's only one piece of a much larger picture. PeerLLM SKILLs is one of a growing series of **consumption capabilities** built on top of the PeerLLM network — the layer where you *use* the network to get real work done. Skills are where we started, but the same decentralized foundation is meant to carry the entire AI lifecycle: the **LLMs** that do the reasoning, the **agents** that orchestrate it, the **MCP servers and tools** they reach for, the **data** they read and produce, and the many other pieces that a real AI system needs to author, evaluate, publish, and run. Each of those is a capability that can live and operate on top of the network rather than inside a handful of centralized data centers.

That's the thread running under all of it: not just decentralized *compute*, but a decentralized place for the whole of AI — its models, its skills, its agents, its data, its tooling — to be built, proven, and shared. PeerLLM SKILLs is the first of those capabilities to make the case in full. It won't be the last.
