# MCP servers

MCP servers I use on every machine, installed globally for each agent. `sync-setup` reads this file to offer installing the missing ones and to suggest adding unlisted ones.

No secrets here. `keyring` is the OS-keyring service holding the token (read by `bin/mcp-headers`), `env` the variable OpenCode reads it from. `-` means no token (none needed, or OAuth on first use).

| name     | url                                | keyring    | env                          |
| -------- | ---------------------------------- | ---------- | ---------------------------- |
| context7 | https://mcp.context7.com/mcp       | context7   | CONTEXT7_API_KEY             |
| github   | https://api.githubcopilot.com/mcp/ | github-pat | GITHUB_PERSONAL_ACCESS_TOKEN |
