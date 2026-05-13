allowed-tools: Bash(git checkout --branch:*), Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(git log:*), Bash(git diff:*), Bash(gh pr create:*), Bash(gh auth status:*)
description: Complete git workflow: commit, push, and open a pull request in one step.

## Context

Current git status: !git status

Current git diff (staged and unstaged changes): !git diff HEAD

Current branch: !git branch --show-current

## Your task

Based on the above changes, execute the complete commit → push → PR workflow.

### Rules

1. **Create a new branch** if currently on `main` or `master`:
   - Generate a short kebab-case branch name based on the changes (e.g. `fix-login-validation`, `feat-add-user-search`).
   - Use `git checkout -b <branch-name>`.

2. **Create a single commit** with an appropriate Conventional Commits message:
   - Analyze the diff and match your repo's commit style.
   - Include `[Claude Code]` attribution in the commit body.

3. **Push the branch** to origin:
   - `git push -u origin <branch-name>`

4. **Create a pull request** using `gh pr create`:
   - Use a descriptive PR title summarizing the changes.
   - Include a PR body with:
     - **Summary**: 1-3 bullet points of what changed.
     - **Test plan**: checklist of how to verify.
     - **Claude Code** attribution.
   - Use `--base main` (or `--base master` depending on the repo default).

### Pre-flight

- Verify `gh` CLI is authenticated: if `gh auth status` fails, tell the user to run `gh auth login` first.
- If there are no changes (clean working tree), stop and tell the user nothing to commit.

### Execution

You have the capability to call multiple tools in a single response. You MUST do all of the above (branch if needed, commit, push, create PR) in a single message batch. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
