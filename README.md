# My Claude Code workflow method

A documented way of working with Claude Code that I've arrived at over the past year of daily use. This is not a framework or a product. It's a personal method I'm writing down because a few people on LinkedIn asked me to, and because writing it down has made me better at using it.

## Why this exists

When I started using Claude Code, I noticed the same pattern most early users hit: impressive first 10 minutes, then degrading output as the session drifted. Vague prompts produced vague code. Long sessions lost context. "Fix this" became a loop.

The method below is what fixed that for me. It comes directly from years of working in regulated manufacturing before I moved into software, where every step in a process has an explicit verification before the next step begins. Applied to AI-assisted coding, the same discipline turns out to work surprisingly well.

I am not claiming this is the best method. It's the method that works for me.

## The three patterns

### 1. Numbered self-contained prompts

Instead of conversational prompting, I write a numbered sequence of prompts up front. Each one is self-contained: it assumes no memory of what came before, includes all context it needs, and ends with an explicit verify step.

A typical sequence for a small feature looks like:

Read CLAUDE.md and the current state of src/intake/. Summarize what
exists. Do not write any code yet. Verify: paste your summary.
Add a new endpoint POST /intake/submit that accepts {name, email, level}.
Validate with Pydantic. Return 201 with the created record.
Verify: run curl against the endpoint with a sample payload and paste
the response.
Write a pytest test for the endpoint covering happy path, missing field,
and invalid email. Verify: run pytest and paste the output.


Three prompts. Each one standalone. Each one verifiable without me having to read the code myself.

### 2. Verify steps

The verify step at the end of each prompt is the single most important part of the method. Without it, Claude Code reports success based on what the code looks like. With it, success is conditional on an observable outcome: a passing test, a working curl, a correct file listing.

Good verify steps are cheap to run and hard to fake:

- `run pytest -x and paste the output`
- `curl the endpoint and paste the response`
- `run git diff --stat and paste the output`
- `list the files in X directory and confirm Y exists`

Bad verify steps are things like "confirm the feature works" or "make sure it's correct." These get reported as successful even when they aren't.

### 3. Coworker delegation

Once a feature is partly built and the verify steps are defined, I can hand off a review-and-fix cycle to Claude Code autonomously. The pattern:
Here is the acceptance criteria for feature X:
[bullet list, specific and verifiable]
Read the current code, identify anything that doesn't meet the criteria,
fix it, run the tests, and report back. If tests fail, keep iterating
until they pass or until you've tried 3 times. Do not ask me clarifying
questions during this pass.

This is where the manufacturing mindset helps. "Coworker" is the right mental model: you're handing off a bounded task with clear acceptance criteria to someone who will execute it, not a conversation partner who needs to be steered at each step.

## What this method is not

- **It's not a replacement for thinking.** The method forces me to decide what "done" means before I start. That's the thinking. The AI does the typing.
- **It's not fully autonomous.** I review every commit. The verify steps are how I review efficiently.
- **It's not fast at first.** Writing numbered prompts takes longer than "just try stuff." It pays off on anything larger than a trivial change.
- **It's not rigid.** I drop it entirely for exploration, spikes, and debugging. It's for *building*, not for *thinking out loud*.

## Anti-patterns I had to unlearn

- **Conversational drift.** "Now also add X. And Y. And actually let's refactor Z while we're here." Each one halves the clarity.
- **Vague verify steps.** "Make sure it works" is not a verify step. "Run the tests and paste the output" is.
- **Skipping the summary step.** Starting with "read the codebase and summarize" saves hours later. When Claude Code misunderstands the existing structure, everything downstream is contaminated.
- **Accepting "I've implemented this" without proof.** The verify step is not optional. If it's not verifiable, it's not done.

## Where I learned this

From the factory floor, honestly. At Panasonic I was authorized to halt production when an anomaly was detected. You didn't "probably fix" a battery cell line. You verified, or you didn't proceed. That discipline transfers to AI-assisted coding more directly than I expected.

## Feedback welcome

I'm still refining this. If you've tried something similar and have a better way, open an issue. If you've tried this and it didn't work for you, I'd genuinely like to hear why.

---

*Built by [Duan Erasmus](https://www.linkedin.com/in/duan-erasmus). Poland, remote.*
