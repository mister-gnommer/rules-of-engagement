---
name: reading-list
ver: 1
description: Lists files and functions for the user to read. Review mode sorts a diff into must-read, skim, and ignore. Learning mode gives an ordered reading path. Use when the user asks what to review or read, or how part of a codebase works.
---

# Reading list

Give the list before any explanation or review comments. Read-only.

Entry format: `path:line` or `path` → `symbol()`, then a one-line reason. Use repo-relative paths. Group functions from the same file under that file.

## Review mode

Scope is the diff under review: the working tree, the branch against its merge-base with the default branch, or the PR the user names. Read the hunks. Don't classify by file name alone. A file goes in the tier of its riskiest hunk. If unsure between two tiers, pick the higher one.

**Must read and understand**: mistakes here are costly or irreversible.
- Auth: login, sessions, tokens, permission and role checks, guards.
- DB: migrations, schema changes, backfills, bulk updates and deletes.
- User data: PII storage, logging, exports, retention, deletion, isolation between tenants.
- Money: paid external APIs (LLMs, SMS, maps, cloud), payments, billing, pricing, quotas. This includes loops, retries, and cron or queue jobs that call them.
- Secrets, env and config, CI/CD, deploy, infrastructure.
- Trust boundaries: input validation, raw SQL, file uploads, deserialization, CORS/CSP, incoming webhooks and their signature checks.
- Effects on real people: emails, notifications, calls to third parties.
- Concurrency: transactions, locks, idempotency.
- Contracts: public API changes, new or upgraded dependencies.
- Anything the author (you included) marked as uncertain, a workaround, or untested.

For each entry, say in one line what to verify, e.g. "the role check runs before the query". If judging a change needs an unchanged file (its caller, the guard it relies on), list that file here and mark it `(unchanged, context)`.

**Just skim**: ordinary logic covered by tests, UI, refactors, glue code. Tests go here too: check that they assert the right thing.

**Can safely ignore**: generated code, lockfiles (new dependencies already show up under must-read through the manifest), snapshots, formatting-only changes, pure renames and moves (confirm with `git diff -M`). Write one line per group with a count, e.g. "lockfile, 14 snapshots".

## Learning mode

Give a reading path in reading order. Include everything needed to understand the topic, with no fixed length:
1. Entry point: route, command, handler, main.
2. Core types and the data model.
3. The main flow, step by step.
4. The part the question is about.

Each entry says what to learn from it ("how a request gets its user"), not what the file contains. Point to functions rather than whole files when a file is large. Mark deep dives as optional.

Search for learning notes left in comments: `git grep -nEi '(//|#|--|/\*|\*)\s*learn(ing)?\b'`. Add every hit that relates to the topic in any way to the entry for its file or function. If a hit's code isn't on the path yet, give it its own entry.

Any explanation that follows uses the same order.

## Both modes in one run

Keep a file in both lists if it belongs to both. In the learning list, tag it with its review tier, e.g. `[review: must-read]`, so the user reads it once with both goals in mind.
