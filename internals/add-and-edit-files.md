# Add and Edit Files

Every file in a Git repo is in one of four states: Untracked, Unmodified, Modified, or Staged. Understanding this cycle is key to making clean commits.

## File States

- `Untracked` — Git has never seen this file. Brand new.
- `Unmodified` — Git tracks it, but nothing changed since the last commit.
- `Modified` — Git tracks it, and you've changed it since the last commit.
- `Staged` — The file (or its changes) is queued for the next commit.

## Flow


Untracked ──git add──> Staged ──git commit──> Unmodified
│
edit file
│
▼
Modified ──git add──> Staged

## Staging Commands

```bash
git add <file>           # Stage one specific file
git add file1 file2      # Stage multiple specific files
git add .                # Stage all changes in current directory and below
git add -A               # Stage all changes in the entire repo (adds, edits, deletes)
```

## Important Detail

> `git add` stages a SNAPSHOT of the file at that moment. If you edit a file after staging, the new changes are NOT staged. You must `git add` again to include them. You can even see the file listed twice in `git status` — once as staged, once as modified.

## Selective Staging

> You don't have to stage everything. Stage only the files that belong to the same logical change, and commit them together. This keeps your commit history clean and focused.

## When to Use This

> Every time you work with files in Git. The add-edit-stage-commit cycle is the most fundamental workflow in Git.