# Initializing a Git Repo & the .git Folder

`git init` turns any regular folder into a Git repository by creating a hidden `.git` folder inside it.

## The Command

```bash
git init    # Creates a .git folder, turning this directory into a Git repo
```

## What's Inside .git

.git/
├── HEAD        # Points to the current branch (e.g. refs/heads/master)
├── config      # Local repo settings (--local level configuration)
├── hooks/      # Scripts that run automatically on Git events
├── objects/    # The database — stores all commits, file versions, and trees
├── refs/       # Branch and tag pointers to specific commits

## The Three Most Important Parts

- `objects/` — Git's database. Every commit, file version, and tree is stored here as a compressed object
- `HEAD` — Tells Git which branch you're currently on
- `refs/` — Where branch names and tags map to specific commit hashes

## Key Takeaway

> The `.git` folder IS the repository. Delete it and your project loses all history, branches, and tracking — it becomes a regular folder again. Never manually edit `.git` unless you know exactly what you're doing.

## When to Use This

> Use `git init` once when starting a brand new project that isn't cloned from a remote. Understanding `.git` helps you troubleshoot and understand how Git works under the hood.