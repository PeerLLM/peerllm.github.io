---
layout: post
title: "Inherited Limits: Making PeerLLM Run on an NVIDIA DGX Spark"
date: 2026-09-20
author: Hassan Habib
categories: [PeerLLM, Hosts, Engineering]
tags: [DGX Spark, NVIDIA, CUDA, ARM64, Hardware, node-llama-cpp]
description: "An NVIDIA DGX Spark arrived on my desk and PeerLLM would not run on it. None of the four things I had to fix were limitations of the machine, and two of them were corrections to earlier fixes. This is what a product inherits from what it depends on, and why the only reliable way to know whether your software runs on a machine is to put it on the machine."
image: /assets/images/dgx-spark-desk.jpg
---

Most houses have a number nobody living in them chose. It is written on the electrical panel, and it was usually decided by an electrician the current owners never met. You can buy any oven you like, but what you can actually run is settled by that panel and the wiring behind it. Nobody thinks of this as a limitation of the oven, because the oven is fine. The limit was inherited, quietly, from a decision made in a different decade for a different family.

Software has panels too, and they are far harder to notice. A product's hardware support is rarely decided by the product alone. It is inherited from whatever the product depends on, and then it is hardened by the choices the product makes about those dependencies, usually without either half being written down. I call the result *Inherited Limits*. An Inherited Limit in its essence is an assumption a dependency supplies and an integration choice turns into a boundary, which means it has at least two authors, and only one of them works here.

This week an NVIDIA DGX Spark arrived on my desk, and PeerLLM would not run on it. Nothing about that turned out to be a limitation of the machine.

<div class="post-image">
  <img src="/assets/images/dgx-spark-desk.jpg"
       alt="The DGX Spark sitting on my desk beside the keyboard, with the PeerLLM host dashboard and the network site open on the monitor behind it, after the work described in this post">
</div>

## 0/ The Wall

The installer refused, and it refused in the most misleading way available. Nine unmet dependencies came back one after another, each of them a common library that the machine obviously had. `libgtk-3-0` not installable. `libnss3` not installable. A wall of red suggesting a badly built package, or a broken system, or both.

The dependencies were fine. Every one of those lines ended in `:amd64`, and the Spark is an ARM machine. When a package declares the wrong architecture, the system dutifully looks for every one of its dependencies in that same wrong architecture, finds none of them, and reports nine failures where there was only ever one. It was a packaging defect, and it came from a single word in a build configuration that was written when my only Linux target was x86, back when the assumption cost nothing at all.

## 1/ Someone Else's Matrix

The deeper limit was one directory listing. PeerLLM runs models through [node-llama-cpp](https://github.com/withcatai/node-llama-cpp), which ships precompiled binaries, one per platform. In the version I shipped, 3.20.0, there was a Linux x64 build with CUDA and a Windows x64 build with CUDA, and there was a Linux ARM build that carried no CUDA at all. So the Spark installed cleanly and then served every request on its processor, while a Blackwell GPU sat beside it drawing five watts.

That listing is not a specification anyone at PeerLLM wrote. It is a maintainer's build matrix, offered freely under a permissive license, and it had quietly become the practical statement of what hardware PeerLLM supports. Practical rather than absolute, because the library can also build from source. My own Electron packaging disables that fallback by default, which is precisely how a matrix meant as a convenience hardened into a boundary.

So I asked for the missing cell rather than only working around it. The request is open as [issue 651](https://github.com/withcatai/node-llama-cpp/issues/651), and my workaround retires when such a build exists and PeerLLM ships it, not on the day the issue closes. That is worth saying plainly, because nobody owed me that build, and the right response to a limit you inherited is to contribute the thing you wished had been there.

That is the shape of an Inherited Limit, and both of its authors are visible in it. The maintainer supplied the assumption, in a directory listing nobody at PeerLLM had any reason to read. I supplied the boundary, in a packaging default that switched off the one escape hatch the library had left open, and I did that years before a machine existed that would need it. Architecture metadata and documentation would have exposed some of this if I had gone looking, and I found it the slower way instead, by owning the hardware and watching the thing fail. The tool itself was honest about the situation, in a sentence worth keeping: CUDA is detected, but using it failed. The drivers were there and the libraries were there. The binary had simply been compiled without them.

## 2/ A Rounded Zero

The second limit was mine, and it is the one most worth passing on. A machine like this shares one pool of memory between its processor and its graphics, so there is no separate video memory, and the accounting has to charge a model against ordinary system memory or the host will admit far more work than it can hold. Weights are only part of that bill. The cache that grows with the conversation, the working buffers, and the rest of the machine all draw on the same pool, and my admission check still counts the weights alone.

The code already knew the general problem. It knew about Apple Silicon, and it knew about integrated graphics that report a very large video memory figure and mean system memory by it. What it looked for was a machine reporting no video memory at all, and the Spark does not do that. The detection library I use, systeminformation, returned a `vram` field of 64 for this GPU, which is neither zero nor a figure I can map to any real pool. Meanwhile `nvidia-smi` on the same machine declines to answer the question at all, reporting memory as not supported. Two tools, two different non-answers, and my code was only ever ready for one of them.

I shipped a fix for this and wrote tests, and the tests passed, and the fix did not work. They passed because I asserted against zero, and zero is what the command line *printed*, since it rounds to gigabytes and sixty-four megabytes rounds away to nothing. I had tested the number on the screen rather than the number from the hardware. The test was real enough. Its fixture simply repeated my assumption back to me, and a fixture built from a guess will confirm the guess every time you run it.

## 3/ Silent Success

The third limit is the difference between a build that fails and a build that lies. Compiling the missing CUDA support required telling the compiler which chip to target, and there is an obvious way to pass that setting, and I used it, and it worked perfectly. The build succeeded, the binary was correct, and the application ignored it completely.

The library, reasonably enough, treats custom build options as part of a build's identity and names the output directory after a hash of them. The application then asks for a binary built with no custom options, computes a different hash, finds nothing that matches, and falls back to the slow one without complaint. Green build, idle GPU, and no error anywhere in the chain, because every component did exactly what it had been told to do. Passing the architecture through `CUDAARCHS`, a standard CMake variable, fixed it, because that channel is not part of the identity. The whole of the fix is four lines, and the last of them is the one I wish I had run first.

```bash
# CUDAARCHS is a standard CMake variable, so it stays out of the build identity.
CUDA_PATH=/usr/local/cuda CUDACXX=/usr/local/cuda/bin/nvcc CUDAARCHS=121   npx --no node-llama-cpp source download --gpu cuda

# Then read the directory name back. A hash suffix means getLlama() never looks there.
ls node_modules/node-llama-cpp/llama/localBuilds   # want: linux-arm64-cuda
```

I want to be careful about how I describe this, because the library's own Electron guide says custom binaries must be built before packaging and that the same options have to be passed when asking for them. It was documented, and I had not read it closely enough. What is true is that I did not catch it until the package was sitting on the machine, doing nothing. There is now a check in the pipeline that fails the build if the output lands in the wrong directory, because the failure mode here is silence, and silence does not page anyone.

## 4/ Found Sideways

The most valuable thing that week was not something I was looking for. Testing the new build meant running the release pipeline without publishing anything, which had never been possible, since pushing a version tag was the only trigger and that also uploads to the world. So I added a way to build without releasing, and the first time I used it, the macOS builds failed.

They had been failing for two weeks. A hosted runner image had rotated, the packaging tool passed the wrong password to the system keychain, and every release since would have produced no Mac installers at all. The error blamed the signing certificate, which was never the problem. Nobody had noticed, and unit tests would not have caught it either, because signing happens during packaging and nothing exercised packaging until a release was already under way. The gap was not missing tests. It was that nobody checks whether a release produced the artifacts it was supposed to produce.

I went looking for whether an ARM machine could run PeerLLM and found instead that no new Mac installer had been published in two weeks. Existing installations kept working, so nothing broke for anyone already running it, but anyone waiting on a fix was waiting for nothing. The bug I was hunting was a good deal smaller than the one I tripped over on the way to it.

## 5/ Honest Numbers

Once the GPU worked, I measured it, and the numbers were not what I had predicted. Before any of them, two things about how they were taken. My harness counted the text chunks the library handed back rather than verified tokens, so every figure here is chunks per second, and nothing about tokens per second follows from it. The clock starts once the model is loaded and the prompt is submitted, and it stops on the last chunk, so it covers prompt processing and the wait for the first chunk as well as the generation after it. With that said, I had expected the GPU to be three to five times faster, and on Qwen2.5 7B Instruct at Q4_K_M, with a 2048 token context and every layer offloaded, I got 43.6 against 22.5 with the GPU disabled, while running four conversations at once the aggregate was 56.7 against 29.3. The chunk rate slightly less than doubled, either way.

What I cannot tell you is why. I have a guess, and a guess is worth a good deal less than an afternoon with a profiler, which is the afternoon this result has now earned. Until I spend it, the honest statement is the measurement with nothing attached to it, because an explanation offered in place of a cause is just a more comfortable way of not knowing.

Then I tried a model built differently. A mixture of experts holds enormous total weights but activates only a slice of them for each token, so its storage requirement follows the total while its per token work follows the slice. The host reported 82.75 gigabytes of weights for a 117 billion parameter model, sat at roughly 64 gigabytes resident, loaded in forty-six seconds, and served comfortably. I have not benchmarked it the way I benchmarked the smaller one, so I am not going to put a number beside it or stack it against the dense model at all. That comparison is the interesting one, and it deserves a measurement rather than an impression. What the architecture suggests, and what I would want to confirm properly, is a machine whose advantage is room rather than pace.

## 6/ What Remains

The Spark now installs, uses its GPU, and accounts for its memory more honestly than it did. That took four fixes, two of which were corrections to earlier fixes, and none of which were about the machine. What stays with me is how much of it needed the hardware present to surface at all. Most of the assumptions in the chain had been true when they were written, and the rest had simply never been challenged. Linux meant x86. Unified memory meant Apple. Zero meant zero. None of them were wrong so much as expired, and nothing in a test suite expires on schedule.

<figure class="post-image shot narrow">
  <img src="/assets/images/toni-spark-host-card.png"
       alt="The host card for Toni Spark on the PeerLLM network: online, Desktop, Linux ARM64, Agentic, a 39 ms response time, 1 percent CPU, RAM at 39 GB of 122 GB and VRAM at the same 39 GB of 122 GB, 108.4K tokens over 13 prompts, and two models held, GLM 4.7 Flash hot and GPT OSS 120B MXFP4 cold">
  <figcaption>
    Read the card carefully. The 39 ms is a health probe answering, not a token being
    produced, and the word beside it is the interface's enthusiasm rather than mine. The
    uptime reads nine minutes, so every figure here is a snapshot of a machine that has
    barely started.
  </figcaption>
</figure>

The machine is on the network now, under the name Toni Spark, and the card is most of this piece rendered as a row of fields. `Linux · ARM64` is a line PeerLLM had no way to print a week ago. The two memory figures read thirty-nine gigabytes of a hundred and twenty-two, the same number twice for system memory and for video memory, because there is one pool and the host has finally stopped pretending otherwise. Before the rounded zero was found, that same card reported a tenth of a gigabyte of video memory and then turned away every model in the catalogue as too large to fit in it.

All of it is in 2.17.4, the version in the corner of that card. The desktop host for this machine is [PeerLLM.Hosts.Apps-linux-arm64-v2.17.4.deb](https://peerllm.blob.core.windows.net/downloads/PeerLLM.Hosts.Apps-linux-arm64-v2.17.4.deb), and the command-line host is `npm i -g peerllm-host-cli`, which compiles the CUDA binary during install on a Grace-class machine and leaves the processor build alone on anything else. The same release also carries orchestrator work that has little to do with this machine and a great deal to do with how much work any host is offered, which is a post of its own.

The hardware you can support is not a decision you make once and record in a document. It is a running total of assumptions supplied by people you have never met and hardened by choices you made without noticing, drifting quietly while you are looking somewhere else. Putting the software on the actual machine is what exposed every item on that total, because not one of the checks I already had was looking for any of them.

So the responsibility is not to keep buying hardware until the assumptions fall out. It is to turn each thing the hardware found into a check that would have found it without the hardware. Two of those checks exist now. The pipeline fails the build when the CUDA output lands in a directory the application will never look in, and the memory test asserts against what the hardware reports rather than what the screen prints. The third does not exist yet, because nothing verifies that a release produced the artifacts it promised, and that is the entire reason two weeks of Mac installers went missing underneath a wall of green builds. Somebody will ask soon whether two of these machines can serve as one host, and I would rather answer that one with a check already written than with another delivery.

---

**Hassan**
