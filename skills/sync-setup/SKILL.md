---
name: sync-setup
ver: 1
description: Syncs this machine's core rules wiring with the rules-of-engagement repo and updates all installed skills. Use only when the user invokes /sync-setup.
disable-model-invocation: true
---

# sync-setup

Checks whether this machine matches the rules-of-engagement repo (`core/` and `skills/`) and fixes the differences the user approves. Work through the steps in order, then report.

Only change things the steps below cover. Before replacing or overwriting any file, show what's there (diff against the repo version) and back it up as `<file>.bak-<yyyyMMdd>`. Never commit or push. Ask first wherever a step says so.

## 1. Repo

Find the local clone (`$REPO` below), stopping at the first hit:

1. The current directory, if its `origin` remote is `mister-gnommer/rules-of-engagement`.
2. The target of an existing core symlink (`~/.claude/CLAUDE.md`, `~/.config/opencode/AGENTS.md`, `~/.codex/AGENTS.md`), up two levels.
3. The path in the `@` import of `~/.claude/CLAUDE.md`, up two levels.

If none of these works, ask the user where the clone is. Don't search the disk or clone it on your own.

- `git fetch`, then check `git status`.
- **Behind remote, clean tree** → `git pull --ff-only`.
- **Behind and dirty, or diverged** → stop and ask.
- **Unpushed commits or uncommitted changes** → note it for the report. `npx skills` installs from GitHub, so skill changes don't reach any machine (including this one) until they're pushed. Core changes reach this machine right away when it uses symlinks.
- **Skill folders with unpushed changes whose `ver` didn't change** → flag them (repo rule: one bump per commit).

## 2. Core rules wiring

Check only tools that are installed (`command -v claude|opencode|codex`, or the config dir exists). Expected state:

| tool        | path                           | should be                                                                     |
| ----------- | ------------------------------ | ----------------------------------------------------------------------------- |
| Claude Code | `~/.claude/CLAUDE.md`          | symlink → `$REPO/core/claude.md`                                              |
| OpenCode    | `~/.config/opencode/AGENTS.md` | symlink → `$REPO/core/core.md`                                                |
| Codex       | `~/.codex/AGENTS.md`           | symlink → `$REPO/core/core.md`                                                |
| Codex       | `~/.codex/config.toml`         | `developer_instructions` = content of `$REPO/core/codex.md` (always a copy)   |

For each path:

- **Correct symlink** → nothing to do.
- **Symlink pointing elsewhere, or broken** → report where it points and ask what to do. It may be intentional.
- **Regular file (copy setup)** → diff it against the repo file. Ask whether to switch to a symlink (recommended) or refresh the copy. Some machines may need copies on purpose, e.g. synced or locked-down dirs.
- **Missing** → suggest creating the symlink. Create it only after the user approves.

Extra checks:

- **Claude `@` import:** `core/claude.md` starts with an absolute `@<path>/core/core.md`. If that path doesn't exist on this machine (different home dir or clone location), Claude silently loses all core rules. Report it and ask how to fix it. Don't edit the repo file on your own, since other machines rely on that path.
- **Codex `developer_instructions`:** if it differs from `core/codex.md`, show the diff and update just that key. Keep the rest of `config.toml` untouched, and use a TOML multi-line string (`"""…"""`).

## 3. Skills

Only update what's already installed. Never add or remove skills in this step.

1. `npx skills update -g -y` updates every installed skill, whatever its source.
2. Spot-check one changed skill: its `ver` in `~/.agents/skills/<name>/SKILL.md` should match the pushed repo version.
3. Compare the folders in `$REPO/skills/` with the lock-file entries (`~/.agents/.skill-lock.json`) whose `source` is `mister-gnommer/rules-of-engagement`. Note any mismatch for the report:
   - in the repo but not installed,
   - installed but gone from the repo.

## 4. Report

Keep it short. Include:

- a table of what was checked, what changed, and what was left as-is (and why),
- anything waiting on the user: unpushed changes, questions you skipped,
- skill mismatches from step 3. Once everything else is done, ask whether to install the missing ones (`npx skills add mister-gnommer/rules-of-engagement -g -y -s <names...> -a <agents...>`, with agents taken from `lastSelectedAgents` in the lock file) or remove the stale ones (`npx skills remove -g -y -s <names...>`). Default: leave them as they are.
- reload notes:
  - Claude Code picks up rules on a new session or `/compact`, and skills on a new session.
  - OpenCode picks up rules on the next request, and skills on restart.
  - Codex picks up both on a new session.
