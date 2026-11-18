# Secrets hardening

What every agent on every machine is kept away from. `sync-setup` reads this file and offers to bring each installed agent's config in line.

No secrets here, only names.

## Credential paths

Reads denied, for file tools and shell commands alike. A trailing `/` marks a directory.

- `~/.ssh/`
- `~/.aws/credentials`
- `~/.gnupg/`
- `~/.docker/config.json`
- `~/.codex/auth.json`
- `~/.local/share/opencode/auth.json`
- `~/.claude/.credentials.json`
- `~/.local/share/keyrings/`
- `~/.netrc`
- `~/.npmrc`
- `~/.git-credentials`
- `~/.kube/`
- `~/.config/gcloud/`

Only `~/.aws/credentials` (long-lived keys) is listed for AWS, so SSO keeps working.

## Project env files

Reads denied in any directory. Names are listed one by one so `.env.example` stays readable; don't shorten them to `.env.*`.

- `.env`
- `.env.local`
- `.env.*.local`
- `.env.development`
- `.env.dev`
- `.env.production`
- `.env.prod`
- `.env.staging`
- `.env.test`

## Token env vars

Removed from the environment of commands the agent runs. AWS variables are left alone on purpose.

- `GITHUB_TOKEN`
- `GH_TOKEN`
- `GITHUB_PERSONAL_ACCESS_TOKEN`
- `CONTEXT7_API_KEY`
- `ANTHROPIC_API_KEY`
- `OPENAI_API_KEY`
- `NPM_TOKEN`

## Network and keyring

Sandboxed commands get network but not the OS keyring: they can install and fetch, but can't use a stored token.

Hosts allowed without asking, where the tool filters by host:

- `github.com`
- `api.github.com`
- `*.githubusercontent.com`
- `registry.npmjs.org`
- `pypi.org`
- `files.pythonhosted.org`

Commands that need a keyring token, run outside the sandbox where the tool can do that:

- `git push`
- `git pull`
- `git fetch`

Commands the agent runs get `GIT_ASKPASS=""` and `GIT_TERMINAL_PROMPT=0`, so a git command without a token fails at once instead of opening a desktop password dialog.
