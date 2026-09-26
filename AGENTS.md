# RULES OF ENGAGEMENT

## description

This repo, even if public, is used by the creator mainly as a way to coordinate
and sync agent setup among different places (work/priv laptop, different
projects and harnesses). It holds two things:

- `skills/` — skills, installed and updated with `npx skills`
  (`npx skills add mister-gnommer/rules-of-engagement`).
- `core/` — global always-applied rules, read from a local clone at
  `~/Prog/projects/rules-of-engagement` (never fetched from the web).

## core/ layout

- `core.md` — tool-agnostic rules. Word them without naming a tool
  ("the agent", not "Claude"). Keep it tight: length dilutes the rules.
- `claude.md`, `codex.md` — per-tool overlays, only for rules that really
  differ between tools (session tracking, ...).

OpenCode has no overlay: v2 loads only one global file and has no include
mechanism (the `instructions` key in `opencode.json` is parsed but not loaded
as of v2.0.14). So its specifics (e.g. model names in subagent tiers) live
in `core.md`, worded as model-family examples, not tool rules.

Wiring (native mechanisms, no build step):

| tool        | wiring                                                                                |
| ----------- | ------------------------------------------------------------------------------------- |
| Claude Code | `~/.claude/CLAUDE.md` → symlink to `core/claude.md`, which `@`-imports `core/core.md` |
| OpenCode    | `~/.config/opencode/AGENTS.md` → symlink to `core/core.md`                            |
| Codex       | `~/.codex/AGENTS.md` → symlink to `core/core.md`; `core/codex.md` content goes to `developer_instructions` in `~/.codex/config.toml` |

Because of the symlinks, edits in `core/` apply immediately (Claude Code on
the next session or `/compact`, OpenCode on the next request). Moving the
clone breaks them, as does changing the absolute `@` path in `claude.md`.

## versioning

No tags/semver: a new version is any pushed commit changing a skill folder.
`npx skills` detects updates via folder hashes; git history is the changelog.

### `ver` field in header

Each skill has `ver` in its mdc header - as described above it is not needed,
but it is useful for human readability.

Agents should keep `ver` bumped (once per commit, not per edit):

- If the agent makes (or helps make) skill changes, bump `ver` automatically,
  without asking.
- If the user only asks to commit and `ver` looks stale, ask whether to bump
  it — never change it on your own in that case.

`core/` files have no `ver`; git history is enough.
