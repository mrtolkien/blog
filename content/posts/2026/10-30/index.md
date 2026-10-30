---
title: "Uraeda"
date: 2026-10-30
series: ["GHOST"]
series_order: 5
---

Once again, I spent too much time building an AI software development workflow.

# Why

I tried Claude Code. Codex. Pi. Kiro. Kimi Code. Copilot. Antigravity. Cursor. OpenCode.
Superpowers. Ponytail.

And while we can feel the models getting better and better, I couldn't help but hate the
"standard" LLM-driven workflow:

{{< timeline >}}

{{< timelineStep icon="comment" >}} Prompt the coding agent about a feature
{{< /timelineStep >}}

{{< timelineStep icon="mug-hot" >}} Wait for it to gather context and respond
{{< /timelineStep >}}

{{< timelineStep icon="circle-question" >}} Start iterating in the chat, respond to
questions one by one, with a shitty interface {{< /timelineStep >}}

{{< timelineStep icon="code" >}} Once the feature is well specced, let the agent build
{{< /timelineStep >}}

{{< timelineStep icon="poo" >}} Be disappointed by the resulting code: too complex,
faulty, poorly organized {{< /timelineStep >}}

{{< timelineStep icon="wand-magic-sparkles" >}} Add more skills, prompts, context, and
rules, and pray it goes well {{< /timelineStep >}}

{{< /timeline >}}

So I sat down, and just like I did for `GHOST`, I thought about how _I_ wanted to
interact with an LLM:

- It needed to be _asynchronous_. I don't want to sit down for 20 minutes while an LLM
  runs tests.
- It needed to be _efficient_. What I hate over everything is having to go back and play
  Q&A after an LLM has spewed a 10k LoC PR. I want good code from the start, following
  my specs, without any uncertainety.
- It needed to capture _intent_. If I was to not look at the code anymore, at least my
  decisions and directions should be stored in the repository, since those are what
  actually generated the code.
- It needed to be _readable_. Walls of text in TUIs don't do it for me. I want context,
  hyperlinks, graphs, formatting, and visual hierarchy.
- It needed to be _robust_. LLMs change every week. Harnesses too. It should be a
  workflow that's understandable from just reading text files in the repo.
- It needed to work on my phone. I don't want to be glued to my PC, waiting for an agent
  to reply to me. I want to be able to evaluate specs and features on the go.

All those things also would have the advantage of working particularly well with local
models:

- Even if running a model is a bit slower locally, it's ok if it's asynchronous
- Being strongly opinionated about the code and workflow pushes agents in the right
  direction

# What

So that's how I ended up creating [uraeda](https://github.com/mrtolkien/uraeda/):

![banner-dark.svg](banner-dark.svg)

![screenshot1](https://github.com/mrtolkien/uraeda/raw/main/docs/images/screenshots/home-desktop-dark.webp)

![screenshot2](https://github.com/mrtolkien/uraeda/raw/main/docs/images/screenshots/spec-phone-dark.webp)

It's a git-driven workflow. All stable state gets comitted to git:

- First, I create a _proposal_ explaining the big picture of the feature or change I
  want to operate
  - `proposal.md`
- The agent works autonomously, in-depth, and researches the current state of the
  codebase, possible issues with the feature, and how it's implemented in other projects
  - `discovery.md`
- If needed (it often is), the user asks for _clarifications_: a list of questions to
  clarify anything that's unclear about the proposal and take important engineering
  decisions. All with necessary context.
  - `clarifications.md`
- After responding to the clarifications, the agent creates a _spec_. It includes user
  stories, important decisions to save, edge cases, mockups, ...
  - `spec.md`
  - [decisions/\[topic\].md](https://github.com/mrtolkien/uraeda/tree/main/specs/decisions)
- At this point, I _will_ have a few comments, and I'll ask for a v2 of the spec
  - `spec-review.md`
- Once the spec is validated, the agent creates a step-by-step implementation _plan_,
  highlighting _waves_ of tasks that can be developped in parallel.
  - `plan.md`
- After the implementation, the agent issues a PR and condenses the information from all
  the flow in a single report, committed to the repository.
  - [done/\[date-slug\].md](https://github.com/mrtolkien/uraeda/tree/main/specs/done)

The decisions and final report are the only artifacts that stay in the repo, as they
will give context beyond code for the future agents.

Locally, the server holds chats session and other info. But you could lose this local
database and it shouldn't be an issue. Everything that matters is in git.

I benchmarked the workflow against other harnesses, having an LLM roleplay the human in
the process, and it _did_ improve results slightly compared to raw Claude Code.

There's still
[tons to improve](https://github.com/mrtolkien/uraeda/blob/main/specs/PRIORITIES.md) but
I'm decently happy with its current state and I'm making it officially v1.0.

# It's complicated

I still have a love-hate relationship with LLMs and their impact on coding.

On the one hand, I finally have been able to build software I've always wanted for
myself. I have tons of vibe-coded apps I use daily, from my own restaurant
recommandation system to bicycle parking spot search engine. Tons of minimal, extremely
fast, simple to use micro-apps. And I can also envision building bigger, better apps
than I ever did.

I can even produce frontends I've always imagined but was never able to write as I hate
Javascript!

On the other hand, all the feeling of accomplishment, success, and flow state I had when
writing code is gone.

I enjoy seeing the final result, and I enjoy having great, tailor-made software.

But I hate what it's done to me. I used to write Rust serverless functions with sub 10ms
startup time. I used to have 100% code coverage on the backends I developed at my
company, all by hand. I used to produce high quality code that people loved working
with.

It's all gone. I don't even know what the code looks like anymore. Half of it is written
in languages I don't even enjoy, and wouldn't be able to evaluate since I didn't have
time to develop a strong taste for it. I've become a project manager with a strong
technical background.

# Next steps

I'll now be using uraead to working on personal projects:

- A turn-based competitive strategy game where both players play simultaneously
- A chat app with modular UI that can easily be self-hosted, federated, and _fast_
- Making a less addictive ui for reading Reddit/Bluesky, optimized for e-ink devices
- A Japanese learning system that picks up from my 12.000 Anki flashcards into something
  more interactive and focused on grammar
- A Go coach that analyses my games and creates Tsumego relevant to the errors I often
  make

It's hard to find time as a dad of two young kids, doing all the cooking, cleaning,
transportation, and of course hours of active time together daily. I hope uraeda will
help me at least make some progress on those projects, here and there, when I have a few
minutes to read the questions.
