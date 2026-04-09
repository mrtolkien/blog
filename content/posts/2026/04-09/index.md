---
title: "Failure"
date: 2026-04-09T13:55:19+09:00
---

It's been about two months since I started working on
[GHOST](https://ghost.tolki.dev/).

The concept is good: "OpenClaw" in Rust, with Nix as the _only_ dependency,
batteries included. An AI assistant that my mom could use and find useful
instantly instead of relying on it extending itself haphazardly.

For the past month, me and my wife have been daily driving it as an AI agent and
it's worked mostly well.

[Plus, the docs look _sick_](https://ghost.tolki.dev/).

But I'm giving up.

# Wrong vibes

When I started the project ([T-KOMA](https://github.com/mrtolkien/t-koma)), I
went _fast_. I had decent core features, but I made some design mistakes that
were compounding and hard to fix with LLMs.

So I re-started from scratch with [GHOST](https://github.com/mrtolkien/GHOST),
and I'm now at a similar point.

I realize this is a feature, not a bug.

LLMs give you the power to move at the speed of light. You feel like you're on
top the world. You iterate through your backlog, quickly test the features, and
move on.

But over time mistakes add up. You discover problematic abstractions. You
discover bugs at foundational levels that you are not sure you can fix reliably.
You encounter issues you didn't even know could exist like SQLite database
corruption.

As you use the software more and more, you discover small bugs that really
should never have been there in the first place. Every usage turns into a
debugging session. All previous features have to be questionned.

And you realize there are [50](https://github.com/zeroclaw-labs/zeroclaw)
[similar](https://github.com/sipeed/picoclaw)
[projects](https://github.com/HKUDS/nanobot) that sprouted up in the two weeks
it took you to get there.

You are faced with three options:

- Start again from scratch with new knowledge
- Keep fighting the 50k+ LoC codebase that grew out of control
- Give up and use one of the other projects

I tried all three, in order.

# What I learned

The thing is, my GHOST is _decent_.

It is legitimately better than the web apps from top LLM providers. Now, my wife
uses it over Gemini as she finds it provides higher quality responses. I also
learned how many moving parts an AI agent has that still is not fully solved.

Let's review them quickly.

## Prompt engineering

Sycophancy and hallucinations are not bugs: they are a core part of LLMs. They
are built that way because their training data is all about pleasing users and
solving problems.

It's harder than it looks to:

- Make LLMs honest
- Make them learn to fail instead of gaslighting the user
- Make them give up when they don't find a satisfactory response
- Make sure they _properly_ cite their sources and never invent information
- Have them look for good, authoritative sources instead of relying on second
  hand snippets or its own "knowledge"

I had to spend a few millions of tokens and hours of end-to-end testing with
LLMs to find the right wording that worked with all big models.

And my model started acknowledging bugs and low quality data instead of pushing
through. It started providing in-depth, well-sourced comparisons when asked
about products. It sounded less like a vending machine and more like an actual
assistant.

The thing is, modern models are very good at following instructions. When given
clear workflows that work against their biases they end up producing much higher
quality output than "raw" usage.

The problem is that AI peddlers don't want higher quality output. Instead of
focusing on long-term positive outcomes for humans, _which it should_, it
focuses on short term gratification.

What I learned is simple: privately funded company **will** optimize for
engagement and user satisfaction, aka, slop and brain damage.

The only way to sidestep it is to work with providers _only_ for LLM access, and
always refuse their tools and apps.

LLMs are extremely powerful systems, but the incentive in place for big
companies to hasten adoptions are detrimental to their apps.

## Knowledge management

There are literally hundreds of systems out there, and I tried building my own
from scratch.

While it seems to work decently... It's far from perfect and it's extremely hard
to test without long-term usage.

That's the issue here: in those new, fast moving feature spaces, it's extremely
hard to evaluate the quality of your implementation. There are some benchmarks
already but I'm not sure they're fully representative of how the systems
actually work live.

What I learned is that I made a lot of assumptions in building a knowledge base
that looked like my Obsidian vault... and I'm still not sure I got it right.

Validation is hard in the LLM space.

## Extensibility

The goal of AI agents is extensibility, but how do you _achieve_ that?

OpenClaw was very painful on that front, being extremely reliant on brew:
running poorly on linux, unable to handle version conflicts on dependencies,
entirely stateful and hard to pin/diff, ...

My approach with lua + nix worked better. But for bigger dependencies like pdf
ingestion I ended up having to fall back to python or containers, adding an
extra layer of complexity.

The more I added to the core binary, the more bloated it got with features that
maybe 1% of users would care about.

For example adding .epub imports wass extremely useful for me to have natural
language semantic search on the books I read, but does it deserve to be part of
the agent for everyone? Clearly not.

So I needed a plugin system, which would likely be WASM if I went there. But
it's not going to happen.

## Providers and pricing

While there are hundreds of providers, performance and pricing are all over the
place:

- Claude Pro offers decent value at 20$/month, but using it "freely" (through
  their API without Claude Code) is against the ToS
- ChatGPT Plus is OK with you using your subscription's tokens as you wish...
  but through and undocumented API. And also I'm not a big ~~Closed~~OpenAI fan.
- Google's pricing is a mess and Gemini CLI always blocked me after 5 minutes of
  usage, even when I was on the pro plan
- Chinese models are getting extremely good but you have to pay per token
  (mostly), and prices can end up relatively high
- Running performant models locally on reasonably priced hardware is still a few
  years away

I still don't have a "clean" solution. If I had to pay for a subscription or
tokens for an AI agent today, I'm not sure which one I'd pick.

## Security

And finally, the reason I gave up.

My GHOST was full YOLO, with full shell access to the host.

When I started actually speccing the security... I realized it was an extreme
amount of work and testing that was already getting solved by so many other
tools.

And if security had been my only issue, I'd have done it. But when I'm running
into small bugs daily... Adding security will just make them worse!

# Keeping the good stuff

Right now I plan to give other AI assistants a shot while using what I learned:

- Using prompt engineering to make it so the AI can't bullshit me
- Trying external memory systems that are _supposed_ to work well
- Relying on their extensibility system to implement the features I want (like
  epub import!)

But unfortunately, I feel like none of the tools popping up really offers what I
had in mind with GHOST.

They all feel more like Obsidian than Notion to me: extremely extensible, but
reliant on extensions. Which means new users need to find out the extensions
they need as they use the software.

Not a great fit for my mom.

# What's next

Tomorrow, I'll do some gardening. Touch some grass.

Then I'll think more about this post mortem.

I am sad to have spend two months on a project and just give up, but at the same
time... I wrote almost zero lines of code, so it does not really feel like "my"
project.

I'm not sure where software development is going. I'm not sure I like this.

But I also think the software of the future will be **modular and open**.

LLMs are extremely good at creating per-user custom interfaces if given a clear
contract and data model.

As different users have different needs, why shouldn't they have different UIs?

I think I'll try working on that next.
