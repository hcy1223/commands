---
description: Revert staged and unstaged changes, with confirmation for new/deleted files.
---

Analyze the current repository status using `git status --porcelain`.

1.  **Identify Deleted Files:**
    *   Find all files marked as deleted (staged `D ` or unstaged ` D`).
    *   If there are deleted files, list them and ask the user "The following files have been deleted. Do you want to restore them? (yes/no)".
    *   If the user says "yes", run `git restore <file>` for each deleted file.

2.  **Identify New Files:**
    *   Find all new files (staged `A ` or untracked `??`).
    *   If there are new files, list them and ask the user "The following new files have been added. Do you want to PERMANENTLY delete them? (yes/no)".
    *   If the user says "yes":
        *   For staged new files (`A `), run `git rm --cached <file>` and then `rm <file>`.
        *   For untracked files (`??`), run `rm <file>`.

3.  **Revert Modified Files:**
    *   Find all modified files (e.g., `M `, ` M`, `MM`).
    *   Revert all these changes without asking. Use `git checkout HEAD -- <files...>` for all modified files identified. This command works for both staged and unstaged modifications.

4.  **Final Report:**
    *   After performing the operations, show the user a summary of what was done (e.g., "Restored 2 files, deleted 3 new files, and reverted 5 modified files.").
    *   If there were no changes to revert, inform the user "There are no changes to revert."
