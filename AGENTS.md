# Global AGENTS.md

## About this file

- `~/.config/amp/AGENTS.md`, `~/.claude/CLAUDE.md`, and `~/.config/AGENTS.md` are all symlinks to `~/git/dotfiles/AGENTS.md` — they are all the same file. Do not attempt to update them separately.
- **This file is public** — it is tracked in a public GitHub repo. Never add secrets, API keys, passwords, internal URLs, or other sensitive information to this file.
- After making changes to this file, commit and push from `~/git/dotfiles`.

## Self-improvement

- When a CLI command, skill, or tool fails and we figure out the correct approach, suggest updating the relevant AGENTS.md (global or project-level) with the fix so future sessions handle it correctly. Keep suggestions small and practical (e.g., a flag to use, a command to run).

## Client-specific

- **VS Code (extension or terminal):** When asked to open a file, open it in VS Code using `code <path>`.
- **VS Code (extension or terminal):** After creating or updating a file, if there's no follow-up question or next step, offer to open it.

## Git

- Combine `git add`, `git commit`, and `git push` in one command unless told otherwise.
- If the user only says "commit" or "add and commit" (without mentioning push), omit the push.

## Notion

- When asked to search or look up something in Notion, use the `notion` skill with `notion-cli search "<topic>" --json` (always use `--json` to avoid truncated IDs).
- If notion-cli auth fails, run `notion-cli auth login` to re-authenticate.

## Slack

- When asked to search or read Slack, use the `slack` skill with `slack-cli`.
- If slack-cli auth fails, run `slack-cli auth login` to re-authenticate.
- If the Slack app is not configured, the user will need to run `slack-cli auth config` first to enter app credentials.

## Linear

- When asked to search or manage Linear issues, use the `linear` skill with the `linear` CLI.
- If auth fails, run `linear auth login` to re-authenticate (requires an API key from linear.app/settings/account/security).

## Shell

- Avoid piping to `head` or `tail` to limit output — it causes SIGPIPE (exit code 141). Instead, use the tool's native flags to limit results (e.g., `gh repo list --limit 80`, `grep -m 10`). If piping is unavoidable, exit code 141 is harmless.

## AWS

- Always use `aws-vault exec <profile> -- <command>` to run AWS CLI commands, never raw `aws`.
- For Athena/data queries, use the `buildkite-data-user` profile.
- If the SSO session has expired, the user will need to re-authenticate interactively via `bik aws generate-config`.
- To query Athena: (1) `aws-vault exec buildkite-data-user -- aws athena start-query-execution --query-string "<SQL>" --work-group primary --region us-east-1`, (2) poll with `get-query-execution` until SUCCEEDED, (3) fetch with `get-query-results`. Use `DESCRIBE` or `SHOW TABLES IN <database>` to discover schemas.
