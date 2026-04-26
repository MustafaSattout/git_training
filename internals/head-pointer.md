# The HEAD Pointer

HEAD is Git's answer to "where am I right now?" It's a pointer that tells Git which branch (or commit) you're currently on.

## How HEAD Works

- In normal work, HEAD points to a **branch**, and the branch points to a **commit**
- Chain: `HEAD ──> branch ──> commit`

```bash
cat .git/HEAD                # See what HEAD points to (e.g. ref: refs/heads/master)
cat .git/refs/heads/master   # See what commit the branch points to
```

## How HEAD Moves

- `git commit` — HEAD stays on the branch. The branch moves forward to the new commit.
- `git checkout <branch>` — HEAD moves to point to the other branch.
- `git checkout <hash>` — HEAD points directly to a commit — detached HEAD state.

## Normal Flow

Before commit:   HEAD ──> master ──> C3
After commit:    HEAD ──> master ──> C4 (new)

- Happens when you checkout a specific commit hash instead of a branch name
- You can look around and even make commits
- But if you switch away without creating a branch, those commits become unreachable and can be lost

```bash
git checkout a1b2c3d    # Enter detached HEAD state
git checkout master     # Return to normal (any detached commits are now orphaned)
```

## Useful Commands

```bash
cat .git/HEAD                    # See what HEAD points to
git log -1 --oneline             # See the current commit
git log --oneline --all --graph  # See HEAD's position in the full history
```

## When to Use This

> Understanding HEAD is essential for branching, merging, rebasing, and troubleshooting. If something feels wrong in Git, the first question is always: "where is HEAD pointing?"