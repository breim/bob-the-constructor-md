# Bob the Constructor CLAUDE.md

<img width="1675" height="939" alt="cover" src="https://github.com/user-attachments/assets/3ed8d7f7-a40f-4014-81f7-5e42398614ba" />

> *"Can we build it? Yes we can!"* — the whole philosophy of this `CLAUDE.md` in one line: when you hand an agent a blueprint, it builds every part of it, not just the parts that feel simple.

## How to use

Drop `CLAUDE.md` into the root of your project:

```bash
curl -O https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/CLAUDE.md
```

Or with `wget`:

```bash
wget https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/CLAUDE.md
```

An identical copy is kept as `AGENTS.md` for tools that read that convention instead (e.g. OpenAI Codex CLI, Cursor):

```bash
curl -O https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/AGENTS.md
```

Both files are kept in sync — if you edit one, mirror the change in the other.

## What's inside

[`CLAUDE.md`](./CLAUDE.md) currently ships 10 rules:

1. **Think Before Coding** (Check The Blueprint First) — surface assumptions and tradeoffs instead of guessing.
2. **Simplicity First** (No Wasted Materials) — minimum code per feature, nothing speculative.
3. **Surgical Changes** (No Demolition Without a Permit) — touch only what the task requires.
4. **Goal-Driven Execution** — turn tasks into verifiable success criteria.
5. **Specs Are The Request** (The Blueprint Is The Job) — a spec is a contract; every item gets built or explicitly flagged.
6. **Write in English** — all code, comments, and docs, regardless of the language the user writes in.
7. **Avoid Code Comments** — self-explanatory code over comments; only explain the non-obvious *why*.
8. **Commits** — Conventional Commits, no `Co-Authored-By` trailers.
9. **Claude CLI Available via Bash** — use it for scriptable, non-interactive sub-tasks.
10. **Code Quality Metrics** — keep complexity, module size, dependency direction, and test coverage on the healthy end.

## Origin

This `CLAUDE.md` was based on the behavioral guidelines published by Andrej Karpathy's skill set, mirrored at:

> https://github.com/forrestchang/andrej-karpathy-skills

The original is a great baseline for keeping coding agents disciplined: think before coding, keep edits surgical, avoid speculative work, and verify against explicit success criteria.

## Why I changed it

The original guidelines bias **heavily toward "do less."** That works well for small, focused tasks (one bug, one function, one refactor), but it broke down in a specific scenario:

> When I passed a `.md` file containing a multi-feature spec, the agent would silently drop features, treating them as "speculative" or "beyond what was asked" — even though the spec *was* the ask.

Rules like *"No features beyond what was asked"*, *"No 'flexibility' or 'configurability' that wasn't requested"*, and *"Every changed line should trace directly to the user's request"* were being applied to individual bullets inside the spec, instead of to the spec as a whole. The agent ended up using "Simplicity First" as justification to cut scope.

## Changes I made

Compared to the [original](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/CLAUDE.md), this file has six more sections than the 4 it started with. The first three changes below are the original fix; the rest were added later as practical guardrails.

### 1. Clarified `2. Simplicity First` (No Wasted Materials)
- Reworded *"If you write 200 lines and it could be 50, rewrite it"* → **"Minimize code per feature, not feature count. If a single feature takes 200 lines and could be 50, rewrite it."**
  - Reason: the original phrasing let the agent reduce *feature count* under the banner of "simplicity."
- Added an explicit boundary at the end of the section:
  > *"Simplicity First governs HOW you implement each requested item, never WHETHER to implement it. Cutting scope is not simplification."*

### 2. Added `5. Specs Are The Request` (The Blueprint Is The Job — new section)
This is the main fix. It establishes that any spec / requirements doc / feature list / `.md` describing what to build is a **contract**, not a suggestion. Specifically:

- Every feature, bullet, or numbered item in the spec must be implemented.
- Silent drops, deferrals, merges, or "phasing" of features are not allowed. If something seems unnecessary, the agent must surface it and ask before skipping.
- "Simplicity First" applies to the implementation of each item, not to which items get implemented.
- Ambiguity on an item triggers a question, not an omission.

### 3. Added a completion checklist requirement
Before declaring a multi-feature task done, the agent must enumerate every item from the spec and its status:

```
- [Feature 1] → done
- [Feature 2] → done
- [Feature 3] → partial — [what's missing and why]
- [Feature 4] → skipped — [reason, surfaced earlier]
```

This forces a re-read of the spec before closing the task — which is exactly the step the agent was skipping before. Informally: *"Can we build it? Yes, all of it."*

### 4. Added `6. Write in English` (new section)
All written output — code, comments, docs, commit messages, PR descriptions — must be in English, regardless of the language the user writes in.

### 5. Added `7. Avoid Code Comments` (new section)
Codifies a clean-code bias: don't restate what the code does, only comment on the non-obvious *why*.

### 6. Added `8. Commits` (new section)
Conventional Commits format, imperative mood, no `Co-Authored-By` trailers.

### 7. Added `9. Claude CLI Available via Bash` (new section)
Use the Claude CLI for scriptable, headless sub-tasks via non-interactive flags — never launch a blocking interactive session from a tool call.

### 8. Added `10. Code Quality Metrics` (new section)
Five proxies for maintainability: cyclomatic complexity, module size, dependency structure, test coverage, and a mutation-testing mindset.

> Sourced from Martin Fowler's talk [*"How AI will change software engineering"*](https://www.youtube.com/watch?v=CQmI4XKTa0U) — these five signals are adapted from how he frames code health there.

## What I kept unchanged

- `1. Think Before Coding` (Check The Blueprint First) — surfacing assumptions and tradeoffs is still valuable.
- `3. Surgical Changes` (No Demolition Without a Permit) — stops drive-by refactors, very useful.
- `4. Goal-Driven Execution` — verifiable success criteria are still the north star.

The original philosophy is still intact. The changes are scoped to one failure mode: **specs being treated as suggestions instead of contracts.**

## Result

After the changes, when I pass a `.md` with N features, the agent:
1. Treats the entire list as the request.
2. Asks before skipping anything.
3. Reports per-feature status before claiming the task is done.

If features still get cut after this, the agent has to do it explicitly — not silently under the cover of "simplicity."

---

Dedicated to my friend [@weedo-dev](https://github.com/weedo-dev) — this project started a while back with him in mind.

