# Bob the Constructor .md

<img width="1675" height="939" alt="cover" src="https://github.com/user-attachments/assets/3ed8d7f7-a40f-4014-81f7-5e42398614ba" />

> *"Can we build it? Yes we can!"*

That line is the whole philosophy. No wasted materials, no demolition without a permit, always up to code, and the full blueprint gets built, including the parts that look optional.

## How to use

Drop `CLAUDE.md` into the root of your project:

```bash
curl -O https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/CLAUDE.md
```

Or with `wget`:

```bash
wget https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/CLAUDE.md
```

`AGENTS.md` is the same file under the name other tools look for, like OpenAI Codex CLI and Cursor:

```bash
curl -O https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/AGENTS.md
```

The two files stay in sync except for rule 9, `Claude CLI Available via Bash`, which `AGENTS.md` drops because it only applies to Claude Code.

## What's inside

[`CLAUDE.md`](./CLAUDE.md) currently ships 11 rules:

1. **Think Before Coding** (Check The Blueprint First). Surface assumptions and tradeoffs instead of guessing.
2. **Simplicity First** (No Wasted Materials). Minimum code per feature, nothing speculative.
3. **Surgical Changes** (No Demolition Without a Permit). Touch only what the task requires.
4. **Goal-Driven Execution**. Turn tasks into verifiable success criteria.
5. **Specs Are The Request** (The Blueprint Is The Job). A spec is a contract. Every item gets built or explicitly flagged.
6. **Write in English**. All code, comments, and docs, whatever language the user writes in.
7. **Avoid Code Comments**. Self-explanatory code over comments. Explain only the non-obvious *why*.
8. **Commits**. Conventional Commits, no `Co-Authored-By` trailers.
9. **Claude CLI Available via Bash**. Use it for scriptable, non-interactive sub-tasks. This one lives in `CLAUDE.md` only.
10. **Code Quality Metrics**. Keep complexity, module size, dependency direction, and test coverage on the healthy end.
11. **Response Format**. Action first, numbered steps, one concrete next step, no preamble or closers.

## Origin

I based this `CLAUDE.md` on the behavioral guidelines from Andrej Karpathy's skill set, mirrored at:

> https://github.com/forrestchang/andrej-karpathy-skills

The original is genuinely good at keeping a coding agent disciplined. Think before coding, keep edits surgical, skip speculative work, verify against explicit success criteria.

## Why I changed it

The original guidelines bias hard toward doing less. That works for a small task: one bug, one function, one refactor. It fell apart the first time I handed an agent a real spec.

> When I passed a `.md` file containing a multi-feature spec, the agent would silently drop features, treating them as "speculative" or "beyond what was asked." The spec *was* the ask.

The agent applied rules like *"No features beyond what was asked"*, *"No 'flexibility' or 'configurability' that wasn't requested"*, and *"Every changed line should trace directly to the user's request"* to individual bullets inside the spec instead of to the spec as a whole. "Simplicity First" became its excuse for cutting scope.

## Changes I made

The [original](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/CLAUDE.md) has 4 sections. This file has 11. The first three changes below are the scope fix. The rest arrived later, each one after something went wrong.

### 1. Clarified `2. Simplicity First` (No Wasted Materials)

The original said *"If you write 200 lines and it could be 50, rewrite it."* That phrasing let the agent cut *feature count* under the banner of simplicity. It now reads:

> *"Minimize code per feature, not feature count. If a single feature takes 200 lines and could be 50, rewrite it."*

The section also ends with an explicit boundary:

> *"Simplicity First governs HOW you implement each requested item, never WHETHER to implement it. Cutting scope is not simplification."*

### 2. Added `5. Specs Are The Request` (The Blueprint Is The Job)

This is the main fix. A spec, requirements doc, feature list, or any `.md` describing what to build is a contract, not a suggestion.

- Every feature, bullet, or numbered item in the spec must be implemented.
- Silent drops, deferrals, merges, and "phasing" are not allowed. If something seems unnecessary, the agent surfaces it and asks before skipping.
- "Simplicity First" applies to the implementation of each item, never to which items get implemented.
- Ambiguity on an item triggers a question, not an omission.

### 3. Added a completion checklist requirement

Before declaring a multi-feature task done, the agent enumerates every item from the spec and its status:

```
- [Feature 1] → done
- [Feature 2] → done
- [Feature 3] → partial — [what's missing and why]
- [Feature 4] → skipped — [reason, surfaced earlier]
```

This forces a re-read of the spec before the task closes. That re-read is exactly the step the agent kept skipping. Informally: *"Can we build it? Yes, all of it."*

### 4. Added `6. Write in English`

All written output goes out in English. Code, comments, docs, commit messages, PR descriptions, whatever language the user writes in.

### 5. Added `7. Avoid Code Comments`

Don't restate what the code does. Comment only when the *why* is non-obvious.

### 6. Added `8. Commits`

Conventional Commits format, imperative mood, no `Co-Authored-By` trailers.

### 7. Added `9. Claude CLI Available via Bash`

Use the Claude CLI for scriptable, headless sub-tasks through non-interactive flags. Never launch a blocking interactive session from a tool call.

### 8. Added `10. Code Quality Metrics`

Five signals of maintainability: cyclomatic complexity, module size, dependency structure, test coverage, and a mutation-testing mindset.

> Sourced from Martin Fowler's talk [*"How AI will change software engineering"*](https://www.youtube.com/watch?v=CQmI4XKTa0U). The five signals are adapted from how he frames code health there.

### 9. Added `11. Response Format`

Shapes every response so the reader can act on it. Lead with the next action, number multi-step work, end with one concrete next step, restate progress each turn, cap visible lists at five, skip preambles and closers. The section also lists when those rules yield: explanations, destructive actions, debug spirals, real ambiguity, and anything the tool's own system prompt requires.

> Adapted from [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) (MIT). The original is a session skill you switch on and off. Here it is always on, and the ADHD framing and the toggle are gone, so the rule reads as a plain output contract.

## What I kept unchanged

- `1. Think Before Coding` (Check The Blueprint First). Surfacing assumptions and tradeoffs is still valuable.
- `3. Surgical Changes` (No Demolition Without a Permit). Stops drive-by refactors, which I care about a lot.
- `4. Goal-Driven Execution`. Verifiable success criteria are what make everything else checkable.

The original philosophy is intact. I changed one thing: the agent no longer gets to treat a spec as a suggestion.

## Result

After the changes, when I pass a `.md` with N features, the agent:

1. Treats the entire list as the request.
2. Asks before skipping anything.
3. Reports per-feature status before claiming the task is done.

Features can still get cut. But now the agent has to say so out loud, instead of quietly filing it under "simplicity."

---

Dedicated to my friend [@weedo-dev](https://github.com/weedo-dev). This project started a while back with him in mind.
