@/home/kris/Prog/projects/rules-of-engagement/core/core.md

# Claude Code Rules

## Subagent Model Tiers
Pick the tier via the Agent tool's `model` param.

## Git Remote Commands
Run `git push`, `git pull` and `git fetch` as a single command from the repo dir: no `&&`, `|`, `cd` or `-C`. Only then do they skip the sandbox and reach GitHub auth.
