---
description: Stage all unstaged files and commit with a message
argument-hint: "[type(scope): subject]"
---

1.  Run the command `git add -A` to stage all unstaged and new files.
2.  After staging, follow all the instructions from the `/commit` command's definition file (`commit.md`) to create the commit. Use `$ARGUMENTS` as the message if provided; otherwise let `commit.md` auto-generate one.
