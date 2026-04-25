# My First Commit

A commit is a permanent snapshot of your project at a specific moment, with a message explaining what changed.

## The Three Areas of Git

Working Directory  ──git add──>  Staging Area  ──git commit──>  Repository
(your files)                    (the box)                    (sealed & saved)

- `Working Directory` — Where you edit files. Git sees changes but doesn't track them yet.
- `Staging Area` — Where you prepare exactly what goes into the next commit.
- `Repository` — The permanent history. Each commit is a saved snapshot.

## Core Commands

```bash
git status                      # See what's changed, staged, or untracked
git add <file>                  # Stage a specific file for the next commit
git add .                       # Stage all changes at once
git commit -m "your message"    # Save the staged snapshot permanently
git log --oneline               # View commit history (one line per commit)
```

## Important Detail

> `git add` stages the EXACT version of the file at that moment. If you edit the file again after adding, you must `git add` it again — otherwise the commit gets the old version.

## When to Use This

> Every single time you want to save progress. The workflow is always: edit → `git status` → `git add` → `git commit`. This is the most fundamental Git cycle.

