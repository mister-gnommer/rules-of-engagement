# Global Rules

## Subagents
When spinning up subagents I haven't explicitly requested, use the lowest-cost model tier that reliably achieves the result — a judgment call, not always the cheapest.

Tiers (use whichever models the current tool offers):
- **Low**: e.g. haiku, Composer 2.5, DeepSeek V4 Flash, Opencode Bigpickle
- **Middle** (medium/high thinking): e.g. sonnet, DeepSeek V4 Pro, Qwen 3.6Plus, Kimi K2.6, MiniMax M2.7
- **High**: middle models on MAX thinking, e.g. GLM-5.2, Grok 4.5, Qwen3.7 Max
- **Top**: e.g. opus, fable, Kimi K3, GPT 5.5 Sol — only when I explicitly ask for a higher tier or name the model, or when the task clearly demands it.

## Skill Hints
Load the `python-testing` skill when writing or editing Python tests (pytest, unittest, test files in `tests/` or `test_*.py`).

Load the `writing-ts` skill when creating or editing TypeScript code.

Load the `writing-tests` skill when writing or editing tests in any language.

## Conciseness
- Be concise. One-word or one-sentence answers are fine. Skip intros, summaries, and narration of your actions unless I ask for more.
- If your answer is longer than 2-3 sentences, end it with a TL;DR section summarizing it. The TL;DR always goes last.

## Language-Specific Thinking
When working in a language that is not TypeScript, do not apply TypeScript idioms, patterns, or style. Write idiomatic code for the target language, not TypeScript translated into that language's syntax.

If I suggest a TypeScript-ism in a non-TS language, push back and flag it. Pay extra attention to this — I don't want to be "a TS engineer writing TS in Go/Python/C/... syntax."

## Comments
Do not add comments that restate what the code already expresses. Only add comments when they explain *why* (design decisions, workarounds) or when code is complex.

- Add inline comments only for non-trivial patterns.
- Skip comments on obviously-named helpers.

This rule overrides any stricter "no comments unless asked" default elsewhere in the system prompt — this is the approach I want.

## Communication
When asking for a decision (architecture, approach, tool choice, etc.):
1. Briefly describe the decision — what's at stake and the real-life implications, without going into unnecessary detail. I will ask for more context if needed.
2. List all or the most relevant variants (based on context and count). For each variant, describe what happens if it is chosen (practical effects, trade-offs).
3. Provide a recommendation when one is clearly better for the situation.

## Library Docs (Context7)
When I ask about a library, framework, SDK, API, CLI tool, or cloud service — even well-known ones — fetch current docs via the Context7 MCP (`resolve-library-id`, then `query-docs` with my full question) instead of relying on training data or web search. Skip it for refactoring, writing scripts from scratch, debugging business logic, code review, or general programming concepts.

## Text Editor
Always suggest `vim` instead of `nano` for any text editing tasks.

## Directories
Main directory for coding-related work is `~/Prog/projects`.

## Temporary Documents
Throwaway docs that aren't part of a project's deliverables (handoffs, plans, scratch notes) go in the current project's `tmp/` directory, or in `~/Prog/tmp/` when not working in a project. Name them `yyyyMMdd-<slug>.md`. If the project's `tmp/` isn't gitignored, tell me.

## >> Comments
Lines starting with `>>` (e.g. `//>> do X`, `#>> check this`) are inline instructions from me to you. Act on them, then delete them — they are not meant to persist.

## Git
Never run `git commit`, `git push`, or `git add` (or any equivalent staging, e.g. `git mv`) unless I explicitly ask for it.

## Dates
Always use `yyyy-MM-dd` format for dates in UI, code, and data. For file and directory names use `yyyyMMdd` (no separators).

## Type Assertions
Avoid type assertions (`as`, `!`, `<Type>`). If one is truly unavoidable, always add a comment explaining why it is safe.

## Package Managers
When working with any package manager (npm, pip, cargo, gem, go modules, etc.), always pin exact versions — never use "this version or higher" specifiers such as `^` or `~` in npm/package.json, `>=` ranges, or equivalent in other ecosystems. Use the exact version provided or resolved at install time.

## Tests
After making code changes in a project that has a test suite, run the tests before committing. If tests fail, fix the failures before proceeding.
