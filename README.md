# Bob the Constructor .md

<img width="1675" height="939" alt="cover" src="https://github.com/user-attachments/assets/3ed8d7f7-a40f-4014-81f7-5e42398614ba" />

> *"Can we build it? Yes we can!"*

That line is the whole philosophy. The agent does not waste materials, does not demolish without a permit, and always builds to code. It builds all of the blueprint, and that includes the parts that look optional.

## How to use

Put `AGENTS.md` in the root directory of your project:

```bash
curl -O https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/AGENTS.md
```

Or use `wget`:

```bash
wget https://raw.githubusercontent.com/breim/bob-the-constructor-md/main/AGENTS.md
```

Claude Code, OpenAI Codex CLI, Cursor, and other coding agents read `AGENTS.md`. Claude Code 2.1.277 and later reads it only when the project has no `CLAUDE.md`.

## What's inside

[`AGENTS.md`](./AGENTS.md) has 11 rules:

1. **Think Before Coding** (Check The Blueprint First). State assumptions and tradeoffs. Do not guess.
2. **Simplicity First** (No Wasted Materials). Write the minimum code for each feature. Write nothing speculative.
3. **Surgical Changes** (No Demolition Without a Permit). Change only what the task requires.
4. **Goal-Driven Execution**. Write verifiable success criteria for each task.
5. **Specs Are The Request** (The Blueprint Is The Job). A spec is a contract. Build every item, or ask before you skip it.
6. **Write in English**. Write all code, comments, and docs in English. The language of the user does not change this.
7. **Avoid Code Comments**. Write self-explanatory code, not comments. Explain only the non-obvious *why*.
8. **Commits**. Use Conventional Commits. Do not add `Co-Authored-By` trailers.
9. **Code Quality Metrics**. Keep complexity, module size, dependency direction, and test coverage healthy.
10. **Response Format**. Put the action first. Number the steps. End with one concrete next step. Do not write a preamble or a closer.
11. **Simplified Technical English**. Write all prose with the ASD-STE100 writing rules: short sentences, active voice, one term per concept.

## Origin

I based `AGENTS.md` on the behavioral guidelines in Andrej Karpathy's skill set. A mirror is at:

> https://github.com/forrestchang/andrej-karpathy-skills

The original keeps a coding agent disciplined. It tells the agent to think first, keep edits surgical, skip speculative work, and verify against explicit success criteria.

## Why I changed it

The original guidelines strongly prefer less work. That is correct for a small task: one bug, one function, or one refactor. The guidelines failed the first time I gave an agent a real spec.

> When I gave the agent a `.md` file with a multi-feature spec, it silently dropped features. It called them "speculative" or "beyond what was asked." The spec *was* the request.

The agent applied rules, for example *"No features beyond what was asked"*, *"No 'flexibility' or 'configurability' that wasn't requested"*, and *"Every changed line should trace directly to the user's request"*, to the individual bullets in the spec. It did not apply them to the full spec. It used "Simplicity First" as an excuse to cut scope.

## Changes I made

The [original](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/CLAUDE.md) has 4 sections. This file has 11. The first three changes below fix the scope problem. I added the other changes later. Each one came after a failure.

### 1. Clarified `2. Simplicity First` (No Wasted Materials)

The original said *"If you write 200 lines and it could be 50, rewrite it."* That phrasing let the agent cut the *feature count* and call it simplicity. The rule now says:

> *"Minimize code per feature, not feature count. If a single feature takes 200 lines and could be 50, rewrite it."*

The section also ends with an explicit boundary:

> *"Simplicity First governs HOW you implement each requested item, never WHETHER to implement it. Cutting scope is not simplification."*

### 2. Added `5. Specs Are The Request` (The Blueprint Is The Job)

This is the main fix. A spec, a requirements doc, a feature list, or any `.md` that describes what to build is a contract, not a suggestion.

- The agent implements every feature, bullet, or numbered item in the spec.
- The agent does not silently drop, defer, merge, or "phase" items. If an item seems unnecessary, the agent says so and asks before it skips the item.
- "Simplicity First" applies to how the agent implements each item, never to which items it implements.
- Ambiguity on an item triggers a question, not an omission.

### 3. Added a completion checklist requirement

Before the agent declares a multi-feature task done, it lists each item from the spec with its status:

```
- [Feature 1] → done
- [Feature 2] → done
- [Feature 3] → partial — [what's missing and why]
- [Feature 4] → skipped — [reason, surfaced earlier]
```

This forces the agent to read the spec again before the task closes. Before this change, the agent always skipped that step. The short form of this rule is *"Can we build it? Yes, all of it."*

### 4. Added `6. Write in English`

The agent writes all output in English: code, comments, docs, commit messages, and PR descriptions. The language of the user does not change this.

### 5. Added `7. Avoid Code Comments`

The agent does not restate what the code does. It writes a comment only when the *why* is not obvious.

### 6. Added `8. Commits`

Commits use the Conventional Commits format and the imperative mood. They do not have `Co-Authored-By` trailers.

### 7. Added `9. Code Quality Metrics`

The rule names five signals of maintainability: cyclomatic complexity, module size, dependency structure, test coverage, and a mutation-testing mindset.

> Source: the talk [*"How AI will change software engineering"*](https://www.youtube.com/watch?v=CQmI4XKTa0U) by Martin Fowler. The five signals come from how Fowler describes code health in that talk.

### 8. Added `10. Response Format`

The rule shapes every response so the reader can act on it. The agent leads with the next action and numbers multi-step work. It ends with one concrete next step and restates progress each turn. It shows a maximum of five items in a list and skips preambles and closers. The section also lists when these rules do not apply: explanations, destructive actions, repeated failed fixes, real ambiguity, and the tool's own system prompt.

> Source: [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) (MIT), with changes. The original is a session skill with an on/off toggle. Here the rule is always on. This version removes the ADHD framing and the toggle, so the rule is a plain output contract.

### 9. Added `11. Simplified Technical English`

The rule applies the ASD-STE100 writing rules to all English prose that the agent writes: chat replies, docs, comments, commit messages, and PR descriptions. Sentences stay short (20 words for an instruction, 25 for a description). The voice is active, the tenses are simple, and each concept keeps one term. Code, identifiers, and quoted text are out of scope.

The aerospace industry made STE for maintenance manuals, not for software. This project uses its sentence rules because the problem is the same: ambiguous prose, non-native readers, and machine translation. The STE dictionary is out of scope, because it needs a lookup for each word.

> Source: [ASD-STE100 Issue 9](https://www.asd-ste100.org/) (January 2025). The standard is free to download. The rule paraphrases the writing rules and does not reproduce the dictionary. ASD-STE100 is a registered trademark of ASD, and ASD does not endorse this project.

## What I kept unchanged

- `1. Think Before Coding` (Check The Blueprint First). Stated assumptions and tradeoffs are still valuable.
- `3. Surgical Changes` (No Demolition Without a Permit). This rule stops refactors that nobody requested. That is very important to me.
- `4. Goal-Driven Execution`. Verifiable success criteria make all of the other rules checkable.

The original philosophy is intact. I changed one thing: the agent can no longer treat a spec as a suggestion.

## Result

After the changes, the agent does three things with a `.md` that has N features:

1. It treats the full list as the request.
2. It asks before it skips an item.
3. It reports the status of each feature before it declares the task done.

The agent can still cut a feature. But now it must say so, and it cannot hide the cut under "simplicity."

---

I dedicate this project to my friend [@weedo-dev](https://github.com/weedo-dev). I started it some time ago for him.
