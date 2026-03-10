# Global AGENTS.md

## About this file

- `~/.config/amp/AGENTS.md`, `~/.claude/CLAUDE.md`, and `~/.config/AGENTS.md` are all symlinks to `~/git/dotfiles/AGENTS.md` — they are all the same file. Do not attempt to update them separately.
- **This file is public** — it is tracked in a public GitHub repo. Never add secrets, API keys, passwords, internal URLs, or other sensitive information to this file.
- After making changes to this file, commit and push from `~/git/dotfiles`.

## Self-improvement

- When a CLI command, skill, or tool fails and we figure out the correct approach, **immediately propose an edit** to the relevant AGENTS.md (global `~/git/dotfiles/AGENTS.md` or project-level) or skill file to record the fix. Don't just mention it — show the exact edit. The goal is to prevent the same mistake in future threads.
- Record fixes in the most specific place: if it's about a particular CLI tool (e.g., `notion-cli`, `slack-cli`, `linear`), update that tool's section in this file or its skill. If it's project-specific, update the project's AGENTS.md.
- Before running a CLI tool, check this file and the relevant skill for known flags and patterns. Don't guess at flags.

## Client-specific

- **VS Code (extension or terminal):** When asked to open a file, open it in VS Code using `code <path>`.
- **VS Code (extension or terminal):** After creating or updating a file, if there's no follow-up question or next step, offer to open it.

## Git

- Combine `git add`, `git commit`, and `git push` in one command unless told otherwise.
- If the user only says "commit" or "add and commit" (without mentioning push), omit the push.
- `gh pr edit` may fail with a GitHub Projects Classic deprecation error (exit code 1) even when the edit partially succeeds. Use `gh api repos/{owner}/{repo}/pulls/{number} --method PATCH -f body="..."` instead.

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

## Buildkite

- A Buildkite MCP server is available in projects that have it configured (`.vscode/mcp.json`). Use it instead of web scraping to query builds, logs, pipelines, and test results.
- When the user shares a Buildkite build URL, extract the org slug, pipeline slug, and build number from the URL to query via MCP tools.
- For build failures, use `get_job_logs` then `search_logs` or `tail_logs` to find errors efficiently — never dump full logs into context.
- If MCP auth has expired, the user will need to re-authenticate via the browser OAuth flow.

## AWS

- Always use `aws-vault exec <profile> -- <command>` to run AWS CLI commands, never raw `aws`.
- For Athena/data queries, use the `buildkite-data-user` profile.
- If the SSO session has expired, the user will need to re-authenticate interactively via `bik aws generate-config`.
- To query Athena: (1) `aws-vault exec buildkite-data-user -- aws athena start-query-execution --query-string "<SQL>" --work-group primary --region us-east-1`, (2) poll with `get-query-execution` until SUCCEEDED, (3) fetch with `get-query-results`. Use `DESCRIBE` or `SHOW TABLES IN <database>` to discover schemas.
