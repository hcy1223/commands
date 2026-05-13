allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git log:*), Bash(git diff:*)
description: Create a git commit with an automatically generated Conventional Commits message based on staged and unstaged changes.

## Context

Current git status: !git status

Current git diff (staged and unstaged changes): !git diff HEAD

Current branch: !git branch --show-current

Recent commits: !git log --oneline -10

## Your task

Based on the above changes, create a single git commit.

### Rules

- If there are no changes (clean working tree), stop and tell the user nothing to commit.
- Analyze the diff and generate a Conventional Commits message that summarizes the changes:
  - Allowed types: `feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert`.
  - Infer the type from the actual changes: `feat` for new features, `fix` for bug fixes, `refactor` for internal restructures, `docs` for doc-only changes, `test` for test-only changes, otherwise `chore`.
  - Match the style of recent commits (look at the `git log --oneline -10` output).
- **Do NOT commit** files that appear to contain secrets: `.env`, `credentials.json`, `*.pem`, `*.key`, `id_rsa`, etc. Warn the user if these are in the diff and skip them.
- Include `[Claude Code]` attribution in the commit body.

### Execution

You have the capability to call multiple tools in a single response. Stage and create the commit using a single message batch. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
