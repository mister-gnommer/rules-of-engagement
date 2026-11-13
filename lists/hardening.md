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
