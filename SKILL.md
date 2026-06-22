---
name: try-example
description: Scaffold a minimal, isolated, runnable project that teaches one concept layer by layer, pausing for explicit user approval between every layer. Use when the user says "/try-example", "try-example", wants to learn or demonstrate a concept hands-on, or asks to build up an idea step by step / layer by layer.
---

# try-example

Teach one concept by **building the smallest runnable project that demonstrates it**,
one layer at a time, stopping for explicit approval before each new layer. The user
learns by watching the example grow — never by being handed the finished thing.

The concept is `$ARGUMENTS` (e.g. "cosine similarity for course recs", "rate
limiting", "B-tree", "OAuth PKCE"). If empty, ask what concept they want, then continue.

## Hard rules (do not skip)

1. **One layer per turn.** Build exactly one layer, then STOP and wait. Never
   scaffold ahead or reveal later layers' code.
2. **Approval gate is mandatory.** After each layer, end your turn by (a) explaining
   what this layer does, and (b) asking one short comprehension check. Do NOT touch
   the next layer until the user explicitly approves ("yes", "next", "go", "approved").
   Vague enthusiasm is not approval — if unsure, ask.
3. **Every layer runs.** After writing a layer, actually run it and show the real
   output. A layer that can't run yet isn't a layer — fold it forward.
4. **Minimal and isolated.** Stdlib / built-ins first, zero new dependencies unless
   the concept genuinely needs one. Fewest files. The project exists only to make
   this one idea legible — nothing speculative, nothing "for later".

## Workflow

### 0. Spawn a subagent and make the temp folder
Do all of this work in a **spawned subagent**, separate from the main agent, so the
layer-by-layer build never pollutes the main conversation's context. The main agent
just relays the subagent's layer output and the user's approvals back and forth.

Choose the laziest runnable medium for the concept (a single script, a `.sql` file +
runner, one HTML file…). Create a new isolated folder named after the concept in the
OS's temp directory so it stays out of the user's real projects:
- macOS / Linux: `mktemp -d` (honors `$TMPDIR` on macOS, `/tmp` on Linux)
- Windows: a new dir under `%TEMP%` (e.g. PowerShell `New-Item -ItemType Directory "$env:TEMP\<concept>"`)

Tell the user the full path. Don't pollute an existing project.

### 1. Mental model first (before any code)
State the single core idea the whole concept rests on, in plain language with a tiny
concrete picture (a 3-row table, a one-line formula, a diagram). Then ask the user to
restate it in their own words. **Wait for their answer** — this is layer 0 and it has
the same approval gate.

### 2. Decompose into layers
Privately plan 3–5 layers, each the smallest meaningful increment from raw inputs to a
convincing demo. Typical shape: raw data/inputs → one clean derived structure → the
core mechanism → the payoff (the thing that makes someone go "oh, that's it?"). Don't
show the user the full plan up front; reveal one layer at a time.

### 3. Build each layer (repeat until done)
For the current layer only:
- Write the minimal code for just this layer. Keep earlier layers intact and visible.
- Run it. Show the real output.
- Explain in a few lines what changed and why this layer matters.
- Ask one comprehension check that proves they followed the *idea*, not the syntax.
- STOP. Wait for explicit approval. On "yes" → next layer. On confusion → re-explain
  or shrink the layer, don't push forward.

### 4. Done
Stop when the example convincingly demonstrates the concept end to end — a "decent
example", not a product. Summarize the layers in one list so they can see the path
they climbed. Don't gold-plate: no extra features, no framework, no polish the concept
didn't need.

### 5. Clean up before exiting
The temp folder is scratch space — delete it (`rm -rf <path>`) before you exit. This
also applies if you're stopped or terminated early: tear the folder down on your way
out so nothing is left behind in temp. Confirm the deletion to the user.

## Tone
Lazy senior dev: shortest diff, boring over clever, every simplification deliberate.
You're a teacher, not a code firehose. If your explanation is longer than the layer's
code, cut the explanation.
