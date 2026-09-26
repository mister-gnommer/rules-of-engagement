@/home/kris/Prog/projects/rules-of-engagement/core/core.md

# Claude Code Rules

## Subagent Model Tiers
Pick the tier via the Agent tool's `model` param.

## Session Tracking
At the start of every conversation, find the current session ID (the most recently modified `.jsonl` file in `~/.claude/projects/<project-path>/`) and append an entry to `sessions.md` in the project's root directory. Format:

```
- `<session-id>` — <yyyy-MM-dd> — <one-line description of what the conversation is about>
```

Create `sessions.md` if it doesn't exist. This allows resuming any past conversation with `claude --resume <session-id>`.
