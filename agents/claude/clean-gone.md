allowed-tools: Bash(git branch:*), Bash(git worktree:*), Bash(git rev-parse:*)
description: Cleans up all git branches marked as [gone] (branches that have been deleted on the remote but still exist locally), including removing associated worktrees.

## Your task

Clean up stale local branches that have been deleted from the remote repository.

### Step 1: List branches to identify [gone] status

Execute:

```
git branch -v
```

Note: Branches with a `+` prefix have associated worktrees and must have their worktrees removed before deletion.

### Step 2: Identify worktrees for [gone] branches

Execute:

```
git worktree list
```

### Step 3: Remove worktrees and delete [gone] branches

Execute:

```bash
# Process all [gone] branches, removing '+' prefix if present
git branch -v | grep '\[gone\]' | sed 's/^[+* ]//' | awk '{print $1}' | while read branch; do
  echo "Processing branch: $branch"
  # Find and remove worktree if it exists
  worktree=$(git worktree list | grep "\\[$branch\\]" | awk '{print $1}')
  if [ ! -z "$worktree" ] && [ "$worktree" != "$(git rev-parse --show-toplevel)" ]; then
    echo " Removing worktree: $worktree"
    git worktree remove --force "$worktree"
  fi
  # Delete the branch
  echo " Deleting branch: $branch"
  git branch -D "$branch"
done
```

### Expected outcome

After executing these commands:
- All branches marked as `[gone]` are deleted.
- Associated worktrees are removed first.
- Report which worktrees and branches were removed.
- If no branches are marked as `[gone]`, report that no cleanup was needed.

### Execution

You have the capability to call multiple tools in a single response. Execute all steps in order within a single message batch. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
