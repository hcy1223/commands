---
description: Commit staged changes
argument-hint: "[type(scope): subject]"
---

Please create a git commit from staged changes only.

Use `$ARGUMENTS` as input message and normalize it to Conventional Commits before committing.

- Run `git diff --cached --quiet`; if no staged changes, stop and tell user to run `git add <file>` or `git add -p`.
- If `$ARGUMENTS` is empty, analyze the staged changes via `git diff --cached` and `git diff --cached --stat`, then generate a Conventional Commits message that summarizes the changes. Commit with that generated message directly. Do NOT ask the user for a message.
- Allowed types: `feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert`.
- If `$ARGUMENTS` already matches `type(scope)?: subject`, keep it as-is.
- If type is missing, infer and prepend one type:
  - `feat`: new user-facing behavior/API.
  - `fix`: bug fix / wrong behavior.
  - `refactor`: internal restructure without behavior change.
  - `docs`: docs-only changes.
  - `test`: tests-only changes.
  - otherwise use `chore`.
- Commit with normalized message: `git commit -m "<normalized_message>" --author="Pi Bot <pi@bot.local>"`.
- On success, show:
  - `git log -1 --pretty=format:'%h %s'`
  - commit file tree via `git show --name-status --pretty='' HEAD`
  - mention what type was auto-added if any.
- On failure, show git error and a clear next step.
- Never auto-stage files.
- Never include unstaged changes in the commit.
