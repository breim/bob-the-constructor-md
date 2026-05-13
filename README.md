# My chaotic CLAUDE.md

<p align="center">
  <img width="540" src="https://github.com/user-attachments/assets/5b2a9c0a-69e3-4891-bf39-b4e1f9e753cb" alt="Imagem do projeto" />
</p>


## How to use

Drop `CLAUDE.md` into the root of your project:

```bash
curl -O https://raw.githubusercontent.com/breim/my-chaotic-claude-md/main/CLAUDE.md
```

Or with `wget`:

```bash
wget https://raw.githubusercontent.com/breim/my-chaotic-claude-md/main/CLAUDE.md
```

## Origin

This `CLAUDE.md` was based on the behavioral guidelines published by Andrej Karpathy's skill set, mirrored at:

> https://github.com/forrestchang/andrej-karpathy-skills

The original is a great baseline for keeping coding agents disciplined: think before coding, keep edits surgical, avoid speculative work, and verify against explicit success criteria.

## Why I changed it

The original guidelines bias **heavily toward "do less."** That works well for small, focused tasks (one bug, one function, one refactor), but it broke down in a specific scenario:

> When I passed a `.md` file containing a multi-feature spec, the agent would silently drop features, treating them as "speculative" or "beyond what was asked" — even though the spec *was* the ask.

Rules like *"No features beyond what was asked"*, *"No 'flexibility' or 'configurability' that wasn't requested"*, and *"Every changed line should trace directly to the user's request"* were being applied to individual bullets inside the spec, instead of to the spec as a whole. The agent ended up using "Simplicity First" as justification to cut scope.

## Changes I made

### 1. Clarified `2. Simplicity First`
- Reworded *"If you write 200 lines and it could be 50, rewrite it"* → **"Minimize code per feature, not feature count. If a single feature takes 200 lines and could be 50, rewrite it."**
  - Reason: the original phrasing let the agent reduce *feature count* under the banner of "simplicity."
- Added an explicit boundary at the end of the section:
  > *"Simplicity First governs HOW you implement each requested item, never WHETHER to implement it. Cutting scope is not simplification."*

### 2. Added `5. Specs Are The Request` (new section)
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

This forces a re-read of the spec before closing the task — which is exactly the step the agent was skipping before.

## What I kept unchanged

- `1. Think Before Coding` — surfacing assumptions and tradeoffs is still valuable.
- `3. Surgical Changes` — stops drive-by refactors, very useful.
- `4. Goal-Driven Execution` — verifiable success criteria are still the north star.

The original philosophy is still intact. The changes are scoped to one failure mode: **specs being treated as suggestions instead of contracts.**

## Result

After the changes, when I pass a `.md` with N features, the agent:
1. Treats the entire list as the request.
2. Asks before skipping anything.
3. Reports per-feature status before claiming the task is done.

If features still get cut after this, the agent has to do it explicitly — not silently under the cover of "simplicity."

