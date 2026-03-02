---
title: "Exorcising the GHOST"
date: 2026-03-02
tags:
  - LLMs
  - GHOST
series: ["GHOST"]
series_order: 2
---

A month ago, I started working on my own AI agent after trying out Clawdbot.

In the process, I made many, _many_, **many** mistakes.

Let's do a quick roundup.

## Vibing a bit too hard

I'm a stay at home dad. My day is split between taking care of my children in
the morning, going to the gym (rarely, I'm fat), studying Japanese, taking care
of the house and garden, cooking, and giving my children full attention from
17:00 to ~22:00 every day.

Please have some mercy on me heavily using LLMs to write this project: if I want
it done in any semblance of time, I need some help.

I've been an LLM user for a long time starting with the Copilot Alpha a few
years back up to
[testing out many services this summer](https://blog.tolki.dev/posts/2025/08-07-llms),
which got me
[roasted pretty hard on HackerNews](https://news.ycombinator.com/item?id=44847741).
But it was the first time I vibe coded _that_ hard on a project.

### Saving information

The first thing I learned is that for some god forsaken reason, tooling and
knowledge for AI agents is still a mess.

We have `AGENT.md`, `mcpServers.json`, `.agents/skills/xyz/SKILL.md`, per-agent
memory systems, and for some reason Claude Code does not want to play nice with
what everyone else uses and uses different paths.

And all that was stacked on top of each other: separation of concerns is bad and
in a month we'll have something new.

To that mess add any notes _you_ want to take and pass to the agent: how do you
track specs? How do you track progress?

LLMs naturally prefer full-text data, but many people use an MCP coupled with
one of the 15 billions Jira clones, meaning you need to context switch to work
on your specs and task tracking.

My first attempt at sidestepping that issue was keeping my own for-agent notes
in
[/docs/dev](https://github.com/mrtolkien/t-koma/blob/main/docs/dev/README.md?plain=1)
and asking my agent in `AGENT.md` to keep it up-to-date. This is pretty much
skills with extra steps, so it sucked.

For my rewrite, I used skills + a `/specs` folder with all the features specs. I
was able to focus on editing markdown files while the coding agent was working
on features, which felt pretty good.

Unfortunately passed the PoC stage, keeping all specs as raw `.md` files became
a pain. I didn't want to delete old specs but they made grepping the repo a
pain.

I don't have a solution for you. AI agent tooling sucks when it feels like it's
not that hard to solve: everything should be in markdown files, and we should
have good deterministic tool to work with them!

### Stupid mistakes

The hardest part of vibing hard and fast is catching subtle assumptions made by
the coding agent.

You see, agents are great at reading specs and creating an ok-ish implementation
plan then iterating based on your feedback.

But they are horrendous at disclosing to _you_ their inner biases as they are so
deeply ingrained in their training data. It seems "natural" to them.

This makes catching "stupid" mistakes extremely hard.

The first one I caught was that Claude Code silently discarded all LLM providers
message blocks that were not a user-facing response or tool call and result.

This means all reasoning was lost between chat steps, which made it extremely
hard to spot: the model wasn't _very_ wrong, it was just _a bit stupid_. Since
it still had the full chat history, its responses were mostly ok. But since it
did not see its own internal reasoning from past steps, it would end getting
more and more wrong over time.

And catching that meant spotting a `_ => ()` in a 700 lines Rust file.

Then I realized why it had done so a few days later. It was not passing a
`reasoning` level to the OpenAI Codex API. As it's unofficial, there's no docs
for it, and even as I passed it the
[full pi mono implementation](https://github.com/badlogic/pi-mono/blob/main/packages/ai/src/providers/openai-codex-responses.ts),
it got it wrong. So since it was not receiving any reasoning blocks anyways!

For about 10 days I was trying to debug extremely complex e2e tests that had
tens of moving parts (web search, web fetch, prompt engineering, TODO injection,
...) without realizing that the most crucial building block was wrong.

I vibed too hard, and got so far, but in the end, I had to think like a clanker.

It got the core _extremely_ wrong and never questioned its assumptions.

### Keeping it simple

The best way to vibe is therefore as simple as it sound: keep it simple, stupid.

Since LLMs make wrong assumptions, you have to make sure each feature or module
in your codebase is as simple as possible.

This makes it easier to audit, review, and also, validate.

I'm not sure it could have made me catch the missing reasoning issue, but at
least it helped me with the rest of the codebase.

Now after every feature I go around the codebase and start a few explore agents
trying to see what we could cull and/or remove. I don't let any module grow too
big, because that's where I can't control it anymore.

## Too much rust destroys even the strongest steel

I went all-in on making the project as rusty as possible: embedded
[SurrealDB](https://surrealdb.com/), MdBook docs,
[browser automation](https://github.com/rust-headless-chrome/rust-headless-chrome),
[web crawling](https://spider.cloud/), ...

And that's where I was reminded of a very hard truth many of us forget, not
_everything_ is better in Rust.

### SurrealDB

SurrealDB, particularly embedded, was a catastrophe. I started with SurrealDB 2,
and ran into multiple memory leaks issues, with inserting 10 1024 dimensions
vectors taking over 16GB of RAM.

I then upgraded to SurrealDB 3 that just got released. The migration was
painful, but Claude made it work. It was supposed to have a much better local
store for embedded runs!

Still over 16GB of RAM used for inserting a few vectors.

I had tried [sqlite-vec](https://github.com/asg017/sqlite-vec). sqlite-vec
worked. I should have stayed with sqlite-vec.

### Docs

I started building the docs with mdbook. Why not? It's in Rust, it's fast, it's
readable.

But it was a bit too dry: no callouts, limited color palettes, syntax
highlighting that had issues with frontmatter in Markdown, ...

So I tried to use mdbook-admonish. Last updated 9 months ago. Broke a lot of
things.

Then I accepted the truth: I should likely use an npm-based doc solution like
Starlight, VitePress, or Docusaurus...

After asking my wife to make a decision for me because I can't be trusted with
frontend, I'm on [Starlight](https://starlight.astro.build/). It looks good. It
has a lot of nice features to make doc readable and comfortable to navigate. It
is relatively simple to style. And it does it all out of the box.

### Browser automation and web crawling

One wall I hit early with my agent was getting clean data from the web.

Many modern websites require Javascript to see the content, and dumping HTML
inside an LLM is a surefire way to overflow context for no reason.

So I tried to use Rust-based browser instrumentation.

While technically possible in Rust through
[headless chrome](https://github.com/rust-headless-chrome/rust-headless-chrome)
and [Spider](https://github.com/spider-rs/spider), it was a huge pain and I
wasted days with Claude Code getting to _good_ results.

Then I ran [Crawl4AI](https://docs.crawl4ai.com/) in Docker and it just worked.

So yeah. I should likely look for simple, well maintained, powerful tools that
can easily be deployed or containerized instead of trying to do it all in Rust
every time.

That's why I use [SearXNG](https://docs.searxng.org/) for the web search of my
ghost btw. It has a JSON mode. It just works.

## Done for the day

### Or will I ever be done?

I'm not sure when I'll be happy with the state of GHOST. I'm writing that while
my vertical screen is home to 3 Claude Code instances hacking away at new core
features, with me tabbing regularly to take a deep look at the specs and a quick
look at the code.

[The doc is coming along nicely](https://ghost.tolki.dev/), and I'm starting to
daily drive my GHOST to decent results.

The next big step is adding a coding agent and that might take a few weeks, even
fully vibing it. Because I won't be happy until I see it pass extremely hard e2e
tests that Claude Code would fail.

### What I'm going for

I got told I was making OpenClaw for autistic people, and honestly, that's sort
of true. While I liked the idea of OpenClaw, I didn't like the meme-y vibe, the
unfettered token usage resulting from poor design decisions, the over-personal
feel, and the focus on _vibes_ rather than performance.

I don't need a friend, I need an assistant that gets shit done.

[I like it when my clankers are assholes instead of sycophants](https://www.psypost.org/sycophantic-chatbots-inflate-peoples-perceptions-that-they-are-better-than-average/?utm_source=dlvr.it&utm_medium=linkedin),
and I think you should too.

So of course my next blog post will be about theories of consciousness and LLMs.
