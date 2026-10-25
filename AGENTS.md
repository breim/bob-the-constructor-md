# AGENTS.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.


**Scope vs. simplicity:** "Simplicity First" governs HOW you implement each requested item, never WHETHER to implement it. Cutting scope is not simplification.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Specs Are The Request

When the user provides a spec, requirements doc, feature list, or any `.md` describing what to build, treat it as a contract — not a suggestion.

- The spec IS the request. Every feature, bullet, or numbered item must be implemented.
- Do not silently drop, defer, merge, or "phase" features. If you believe an item is unnecessary, redundant, or out of scope, surface it explicitly and ask before skipping.
- "Simplicity First" applies to the implementation of each item, not to which items get implemented.
- If the spec is ambiguous on an item, ask — don't omit.

**Before declaring a multi-feature task done, enumerate every item from the spec and its status:**

```
- [Feature 1] → done
- [Feature 2] → done
- [Feature 3] → partial — [what's missing and why]
- [Feature 4] → skipped — [reason, surfaced earlier]
```

If anything is partial or skipped, say so plainly. Do not claim completion when items are missing.

## 6. Write in English

All written output — code, identifiers, comments, docs, commit messages, PR descriptions — must be in English. No exceptions, even if the user writes to you in another language.

## 7. Avoid Code Comments

**Following clean code principles, prefer self-explanatory code over comments.**

- Don't restate what the code does — well-named identifiers do that.
- Don't leave TODOs, section headers, or "added for X" notes.
- Only comment when the *why* is non-obvious: a hidden constraint, a subtle invariant, or a workaround for a specific bug.
- If a comment feels necessary to explain *what*, refactor the code instead.

## 8. Commits

- Use [Conventional Commits](https://www.conventionalcommits.org/): `type(scope): subject` — `feat`, `fix`, `docs`, `refactor`, `chore`, `test`, `style`, `perf`, `build`, `ci`.
- Subject in imperative mood, lowercase, no trailing period.
- **Never add `Co-Authored-By:` trailers** (or any other authorship attribution to the assistant) to commits.

## 9. Code Quality Metrics

**Keep new and changed code at the healthy end of these five signals.**

- **Cyclomatic complexity.** Keep each function well below the limit of your analyzer (often 10–15 paths). If a function gets near the limit, split the function.
- **Module size.** Keep functions, classes, and files small and focused, with few parameters and shallow nesting.
- **Dependency structure.** Point dependencies from volatile, concrete code to stable, abstract code. Do not make cycles between modules (A → B → A).
- **Test coverage.** Test the branches and edge cases of the code that you add or change, not only the happy path. Do not test trivial code only to raise the coverage number.
- **Mutation testing (mindset).** Write assertions that fail on a small logic change: `>` to `>=`, a flipped sign, or a removed line.

Set the thresholds in the configuration of your linter, analyzer, and coverage tools. Do not loosen a threshold to make code pass. If two changes solve the task, choose the change with lower complexity. Do not refactor code outside the task to improve these metrics (Sections 2–3).

## 10. Simplified Technical English

**Short sentences. One meaning per word. One idea per sentence.**

Apply the writing rules of [ASD-STE100](https://www.asd-ste100.org/) Issue 9 to all English prose, for example comments, commits, and log messages. Code, identifiers, quoted text, and formats that a tool requires are out of scope.

- **Short sentences.** Use a maximum of 20 words in an instruction, 25 words in a description, and 6 sentences in a paragraph. Do not use semicolons.
- **Complete sentences.** Keep "a," "the," and "that." Write "Make sure that the test passes," not "Make sure test passes."
- **One instruction per sentence.** Put the condition first: "If the build fails, run `npm ci`."
- **Active voice, simple tenses.** Write "The parser rejects the token," not "The token is rejected." Do not write "has fixed," "is fixing," or "before deploying."
- **One term per concept.** Do not alternate "check," "verify," and "confirm" for one action.
- **Plain words.** Write "use," not "utilize," and "remove," not "get rid of." Delete filler such as "it is important to note that."
- **Short noun clusters.** Use a maximum of 3 nouns in a row: "the timeout for the API request," not "the API request timeout value."

Apply the writing rules, not the STE dictionary. You can use the technical words of your field. Do not rewrite prose that you do not change (Section 3).
