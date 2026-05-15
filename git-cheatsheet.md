# Git & GitHub Cheat Sheet

A personal reference covering everything from version control basics to Git internals. Built from hands-on learning — every command here has been used, tested, and understood.

---

## 📑 Table of Contents

1. [📚 Concepts & Theory](#concepts-theory)
2. [🔧 Setup & Configuration](#setup-configuration)
3. [🚀 Starting a Project](#starting-a-project)
4. [📝 The Commit Workflow](#the-commit-workflow)
5. [📂 File Operations](#file-operations)
6. [🔍 Inspecting & Status](#inspecting-status)
7. [🔄 Comparing Changes (Diff)](#comparing-changes-diff)
8. [↩️ Undoing Changes](#undoing-changes)
9. [🚫 Ignoring Files](#ignoring-files)
10. [🌐 Working with Remotes](#working-with-remotes)
11. [🆘 Help & Documentation](#help-documentation)
12. [🔬 Git Internals](#git-internals)
13. [💬 Commit Messages](#commit-messages)
14. [🚨 Troubleshooting](#troubleshooting)
15. [⚡ Quick Reference](#quick-reference)
16. [⚠️ Common Mistakes](#common-mistakes)

---

<a id="concepts-theory"></a>
## 📚 Concepts & Theory

### Version Control System (VCS)

A system that tracks changes to files over time, enabling collaboration, history tracking, and safe recovery from mistakes.

**Three types:**

| Type | How It Works | Single Point of Failure | Example |
|------|-------------|------------------------|---------|
| Local | Tracks changes on one machine | Yes — your hard drive | RCS |
| Centralized (CVCS) | Central server holds everything | Yes — the server | SVN, CVS |
| Distributed (DVCS) | Every dev has a full copy | No | **Git**, Mercurial |

### Git vs GitHub

- **Git** — The version control engine. Runs locally in your terminal.
- **GitHub** — A cloud platform that hosts Git repos and adds collaboration features (PRs, issues, CI/CD).

> Git commands work identically across GitHub, Bitbucket, and self-hosted servers.

### The Git Landscape

| Tool | What It Is |
|------|-----------|
| Git | The version control system itself |
| GitHub | Cloud hosting + largest dev community |
| Bitbucket | Atlassian's cloud platform, integrates with Jira |
| Stash (now Bitbucket Server/Data Center) | Self-hosted Atlassian Git platform |

### Why Git Won

- **Snapshots, not deltas** — stores the full project state at each commit
- **Distributed** — every clone is a full backup
- **Speed** — most operations are local, nearly instant
- **Cheap branching** — branches take milliseconds to create
- **Data integrity** — everything is checksummed with SHA-1

---

<a id="setup-configuration"></a>
## 🔧 Setup & Configuration

### Installation

```bash
# Ubuntu/Linux
sudo apt update && sudo apt install git

# Mac
xcode-select --install        # Simplest option
brew install git              # If you use Homebrew

# Windows 11
# Download from https://git-scm.com/download/win
```

### Verify Install

```bash
git --version
```

### Configuration Levels

| Level | Flag | Applies To | Stored In |
|-------|------|-----------|-----------|
| System | `--system` | All users on the machine | `/etc/gitconfig` |
| Global | `--global` | You, across all repos | `~/.gitconfig` |
| Local | `--local` | One specific repo | `.git/config` |

> **Priority:** Local > Global > System

### Essential First-Time Setup

```bash
git config --global user.name "Mustafa Sattout"
git config --global user.email "your@email.com"
```

> ⚠️ Email **must match** your GitHub email — otherwise commits won't link to your profile or count in your contribution graph.

### Useful Settings

```bash
git config --global init.defaultBranch main      # Default branch for new repos
git config --global core.editor "code --wait"    # Set VS Code as editor
```

### Viewing Config

```bash
git config --list                # All current settings
git config user.name             # Check a specific setting
```

### Tab Completion

Press **Tab** after partial commands, branch names, or flags to autocomplete.

```bash
# If not working on Linux/Bash, enable manually:
source /usr/share/bash-completion/completions/git
```

---

<a id="starting-a-project"></a>
## 🚀 Starting a Project

### Create a New Repo

```bash
git init                         # Turn current folder into a Git repo
```

### Clone an Existing Repo

```bash
git clone https://github.com/user/repo.git              # Clone via HTTPS
git clone git@github.com:user/repo.git                  # Clone via SSH
git clone https://github.com/user/repo.git my-folder    # Clone into custom folder
```

> When you clone, the `origin` remote is set up automatically. Ready to push and pull immediately.

### Init vs Clone

| Use Case | Command |
|----------|---------|
| Brand new project from scratch | `git init` |
| Get a copy of an existing remote repo | `git clone <url>` |

---

<a id="the-commit-workflow"></a>
## 📝 The Commit Workflow

### The Three Areas

```
Working Directory  ──git add──>  Staging Area  ──git commit──>  Repository
   (your files)                    (the box)                    (.git folder)
```

### Core Workflow

```bash
git status                       # See what changed
git add filename.md              # Stage one file
git add .                        # Stage everything in current dir + below
git add -A                       # Stage everything in entire repo (incl. deletes)
git commit -m "feat: add login"  # Commit staged changes
git log --oneline                # View commit history
```

### File States

| State | Meaning |
|-------|---------|
| Untracked | Git has never seen this file |
| Unmodified | Tracked, no changes since last commit |
| Modified | Tracked, changed since last commit |
| Staged | Queued for the next commit |

> ⚠️ `git add` stages a **snapshot** of the file at that moment. Edit it again? You must `git add` again — otherwise the new changes won't be in the commit.

### Single-Step Add + Commit

```bash
git commit -am "feat: update something"     # Stage all modified/deleted tracked files + commit
```

| Flag | What It Does |
|------|-------------|
| `-a` | Automatically stage all **modified** and **deleted** tracked files |
| `-m` | Provide the commit message inline |
| `-am` | Combination of both |

> ⚠️ `-am` does **NOT** stage **untracked** (brand new) files. For mixed changes, use `git add -A && git commit -m "msg"` instead.

| Command | New Files | Modifications | Deletions |
|---------|-----------|--------------|-----------|
| `git commit -am "msg"` | ❌ | ✅ | ✅ |
| `git add . && git commit -m "msg"` | ✅ | ✅ | partial |
| `git add -A && git commit -m "msg"` | ✅ | ✅ | ✅ |

### Preview Before Staging (Dry-Run)

```bash
git add --dry-run .              # Preview what would be staged (long form)
git add -n .                     # Same — short form
git add -nv .                    # Preview with verbose output (shows modifications too)
git add -n "*.md"                # Preview which files match a pattern
```

> Dry-run only **previews** — nothing is actually staged. Use it before `git add .` or `git add -A` to confirm what will be touched, especially with wildcards or in unfamiliar repos.

### Don't Confuse

- `git add .` — stages everything in current directory and below
- `git add -A` — stages everything in the entire repo (including deletions)
- `git commit -am` — stages tracked changes only (skips new files)

---

<a id="file-operations"></a>
## 📂 File Operations

### Move or Rename

```bash
git mv old-name.md new-name.md                       # Rename
git mv basics/file.md internals/file.md              # Move to another folder
git mv basics/old.md internals/new.md                # Rename + move
```

> `git mv` does three things at once: moves the file on disk, stages the deletion of the old path, and stages the addition of the new path.

> ⚠️ The file must already be tracked (committed at least once) before `git mv` will work.

### Delete

```bash
git rm filename.md                # Delete from disk AND stage the deletion
git rm --cached filename.md       # Stop tracking but KEEP the file on disk
```

> Use `--cached` when you accidentally committed a sensitive file (like `.env`) — it removes it from Git tracking without deleting your local copy.

### Don't Confuse

- `git rm <file>` — deletes the file AND stages the deletion
- `git rm --cached <file>` — only removes Git tracking; file stays on disk
- `rm <file>` (regular) — deletes from disk, but Git doesn't stage the change automatically

---

<a id="inspecting-status"></a>
## 🔍 Inspecting & Status

### Check Status

```bash
git status                       # Full verbose output with hints
git status -s                    # Short — one line per file
git status --short               # Same as -s
git status -sb                   # Short + branch info
```

`git status` shows three sections:

| Section | Meaning |
|---------|---------|
| Changes to be committed | Files in the staging area, ready to commit |
| Changes not staged for commit | Tracked files modified but not yet staged |
| Untracked files | Files Git has never seen — won't be committed unless added |

### Reading the Short Output

Two columns: `[staging area][working directory]`

| Code | Meaning |
|------|---------|
| `A ` | New file, staged |
| `M ` | Modified, staged |
| ` M` | Modified, NOT staged |
| `MM` | Staged AND modified again after staging |
| `AM` | New file staged AND modified again after staging |
| `D ` | Deleted, staged |
| ` D` | Deleted, NOT staged |
| `??` | Untracked |
| `R ` | Renamed, staged |

> Run `git status` constantly — before every `add`, before every `commit`. It's safe (never changes anything) and catches mistakes before they become commits.

### View History

```bash
git log                          # Full history (verbose)
git log --oneline                # One line per commit (short hash + message)
git log --oneline --all --graph  # Visualize all branches with ASCII graph
git log -1 --oneline             # Just the latest commit
git log -1 --format="%H"         # Full hash of latest commit
git log -1 --format="%h"         # Short hash of latest commit
```

---

<a id="comparing-changes-diff"></a>
## 🔄 Comparing Changes (Diff)

### The Four Diff Modes

```bash
git diff                         # Working directory vs Staging area
git diff --staged                # Staging area vs Last commit
git diff --cached                # Same as --staged (alternate spelling)
git diff HEAD                    # Working directory vs Last commit (all changes)
git diff <commit1> <commit2>     # One commit vs another commit
```

### Pipeline View

```
Working Directory  ──git diff──>  Staging Area  ──git diff --staged──>  Last Commit (HEAD)
        │                                                                       ▲
        └────────────────────────── git diff HEAD ─────────────────────────────┘
```

### Diff Mode Reference

| Command | Compares | Use Case |
|---------|----------|----------|
| `git diff` | Working dir ↔ Staging | "What did I change but not stage?" |
| `git diff --staged` | Staging ↔ HEAD | "What's about to be committed?" |
| `git diff HEAD` | Working dir ↔ HEAD | "Total changes since last commit, staged or not" |
| `git diff <c1> <c2>` | Two commits | "What changed between these two points?" |

### Reading Diff Output

```
diff --git a/README.md b/README.md     # Comparing two versions
--- a/README.md                         # OLD version (baseline)
+++ b/README.md                         # NEW version (current)
@@ -1,3 +1,5 @@                        # Hunk: lines 1-3 → lines 1-5
 # Git Training                         # Unchanged (space prefix)
+New line added                         # Added (+ prefix)
-Old line removed                       # Removed (- prefix)
```

| Symbol | Meaning |
|--------|---------|
| `+` | Line was added |
| `-` | Line was removed |
| ` ` (space) | Unchanged context line |

### HEAD Shortcuts

| Reference | Meaning |
|-----------|---------|
| `HEAD` | Latest commit on current branch |
| `HEAD~1` or `HEAD^` | One commit before HEAD |
| `HEAD~2` | Two commits before HEAD |
| `HEAD~N` | N commits before HEAD |

### Useful Variations

```bash
git diff <file>                  # Just one specific file
git diff --stat                  # Summary — files changed + line counts
git diff --color-words           # Word-level highlighting instead of line-level
git diff HEAD~5 -- README.md     # How a specific file changed in last 5 commits
git diff master feature-branch   # Compare two branches
```

> The `--` in `git diff HEAD~5 -- README.md` separates revisions from filenames, so Git knows you mean a file (not a branch with that name).

### Most Important Habit

> Run `git diff --staged` **BEFORE every `git commit`**. It shows exactly what's about to be committed. A 5-second check saves hours of "why did that change get in there?"

### Don't Confuse

- `git diff` — what you've changed but **NOT** staged
- `git diff --staged` — what you've staged, ready to commit
- `git diff HEAD` — **everything** different from last commit (staged + unstaged combined)

---

<a id="undoing-changes"></a>
## ↩️ Undoing Changes

### Rollback File Changes

Which command depends on **where** the change is — working directory, staging, or both.

```bash
git restore <file>                      # Discard unstaged changes (working dir)
git checkout -- <file>                  # Same thing, older syntax
git restore --staged <file>             # Unstage but keep edits
git reset HEAD <file>                   # Same thing, older syntax
git restore --staged --worktree <file>  # Discard BOTH staged + unstaged for one file
git reset --hard HEAD                   # Wipe ALL uncommitted changes in the repo
```

### Decision Table

| Situation | Command | Result |
|-----------|---------|--------|
| Modified, not staged → discard | `git restore <file>` | Reverts to last committed version |
| Staged → unstage but keep edits | `git restore --staged <file>` | Moves from staging back to modified |
| Staged + modified → discard all | `git restore --staged --worktree <file>` | Reverts completely |
| Throw away everything | `git reset --hard HEAD` | All uncommitted changes wiped |

### Mental Model

- `git restore <file>` — affects **working directory**
- `git restore --staged <file>` — affects **staging area**
- `git restore --staged --worktree <file>` — affects **both**

> ⚠️ `git restore <file>` and `git reset --hard HEAD` are **destructive**. Discarded changes are gone forever — no undo. Always run `git status` and `git diff` first to confirm what you're throwing away.

### Amend the Last Commit

Rewrite the most recent commit — fix the message, add forgotten changes, or both. **Replaces** the commit with a new one (new hash).

```bash
git commit --amend -m "fix: corrected message"   # Replace message
git commit --amend --no-edit                      # Add staged changes, keep message
git commit --amend                                # Open editor for bigger rewrite
```

**Common workflow — forgot to add a file:**

```bash
git add forgotten-file.css
git commit --amend --no-edit
```

| Situation | Safe to amend? |
|-----------|---------------|
| Local commit, never pushed | ✅ Yes |
| Pushed to a personal branch only you use | ⚠️ Risky — needs force-push |
| Pushed to master/main/shared branch | ❌ Never — use `git revert` instead |

> ⚠️ Amend creates a **new commit with a new hash**. Never amend a pushed commit on a shared branch — others who pulled the old hash will have conflicting history.

### Rollback a Commit — Reset vs Revert

Two ways to undo commits. **Reset rewrites history. Revert preserves it.**

#### git reset — Rewrite History

```bash
git reset --soft HEAD~1     # Undo commit, keep changes STAGED
git reset HEAD~1             # Default --mixed: changes go back to WORKING DIR
git reset --hard HEAD~1      # Undo commit, DELETE all changes
git reset --hard a1b2c3d     # Reset to a specific commit (wipes everything after)
```

| Mode | Branch | Staging | Working Dir |
|------|--------|---------|-------------|
| `--soft` | Moves back | Keeps changes | Untouched |
| `--mixed` (default) | Moves back | Empty | Keeps changes |
| `--hard` | Moves back | Empty | Wiped |

#### git revert — Safe Undo (creates a new commit)

```bash
git revert HEAD              # Revert the most recent commit
git revert a1b2c3d           # Revert a specific commit
git revert --no-edit HEAD    # Skip the editor for the message
git revert --no-commit HEAD  # Stage the revert without auto-committing
```

```
Before:   C1 ── C2 ── C3 (HEAD)
After:    C1 ── C2 ── C3 ── C2_revert (HEAD)
```

#### The Golden Rule

> 🚨 **Never `git reset` commits that have been pushed to a shared branch.** The rewrite will conflict with anyone who has the original. Use `git revert` instead — it adds a new commit on top, working perfectly with shared workflows.

| Situation | Use |
|-----------|-----|
| Local commit, never pushed | `reset` (any mode) |
| Personal branch, only you use | `reset` (with force-push) |
| Pushed to shared branch | `revert` always |

### Don't Confuse

- `git reset` — rewrites history; commits become orphaned
- `git revert` — preserves history; creates a new "undo" commit
- `git restore` — undoes file changes only, doesn't touch commits
- `git clean` — removes untracked files only, doesn't touch tracked work

### Remove Untracked Files (git clean)

Sweep away files Git doesn't track — leftover scratch files, build artifacts, failed experiments. Tracked and gitignored files stay safe by default.

```bash
git clean -n            # Preview what would be deleted (dry run)
git clean -nd           # Preview including untracked directories
git clean -f            # Actually delete untracked files
git clean -fd           # Delete untracked files AND directories
git clean -fdx          # Delete untracked + gitignored files (nuclear)
git clean -i            # Interactive mode — pick what to delete
```

| Flag | Purpose |
|------|---------|
| `-n` | Dry run — preview only |
| `-f` | Force — required to actually delete |
| `-d` | Include directories |
| `-x` | Include gitignored files |
| `-i` | Interactive selection |

| What's Affected | By `git clean`? |
|-----------------|-----------------|
| Untracked files | ✅ With `-f` |
| Untracked directories | ✅ With `-fd` |
| Gitignored files | ✅ Only with `-x` |
| Modified tracked files | ❌ Never |
| Staged changes | ❌ Never |
| Committed files | ❌ Never |

> ⚠️ `git clean -fdx` deletes everything untracked **including** `node_modules/`, `.env`, and other gitignored files. There is no undo. Always run `git clean -n` first to preview.

---

<a id="ignoring-files"></a>
## 🚫 Ignoring Files

### What .gitignore Does

A file in your repo containing patterns. Git skips matching untracked files — they don't appear in `git status`, can't be committed, won't be tracked.

### Pattern Syntax

| Pattern | Matches |
|---------|---------|
| `secrets.txt` | File named exactly `secrets.txt` anywhere |
| `*.log` | Any file ending in `.log` |
| `build/` | Folder `build` and everything inside |
| `/config.json` | Only `config.json` at repo root |
| `**/cache` | Any folder named `cache` at any depth |
| `!keep.log` | Don't ignore `keep.log` (re-include) |
| `# comment` | Comment line — ignored by Git |

### Example .gitignore

```bash
# Dependencies
node_modules/

# Environment variables
.env
.env.local

# Build outputs
dist/
build/

# Logs
*.log

# Editor & OS files
.vscode/
.DS_Store

# Keep one specific log
!important.log
```

### Creating One

```bash
touch .gitignore                  # Create empty file
git add .gitignore
git commit -m "chore: add .gitignore"
```

The `.gitignore` file itself IS tracked — that's how teammates inherit the same rules when they clone.

> ⚠️ `.gitignore` only affects **untracked** files. If a file is already being tracked, adding it to `.gitignore` won't stop tracking. Use `git rm --cached <file>` to remove it from tracking while keeping it on disk.

### Track Empty Directories

> 🚨 Git does NOT track empty folders. A directory only exists in Git if it contains tracked files.

**Solution: add a placeholder file** (community convention is `.gitkeep`):

```bash
mkdir logs
touch logs/.gitkeep
git add logs/.gitkeep
git commit -m "chore: keep logs directory"
```

`.gitkeep` is **not** a Git feature — just a recognizable naming convention.

### Pattern: Folder Tracked, Contents Ignored

```bash
# In .gitignore:
logs/*
!logs/.gitkeep
```

| Pattern | Effect |
|---------|--------|
| `cache/` | Ignore the entire folder — placeholders won't help |
| `cache/*` | Ignore CONTENTS only — folder can exist via placeholder |

The trailing `/` vs `/*` matters: `/` ignores the folder itself, `/*` only ignores what's inside.

### Ignore Already-Tracked Files

`.gitignore` is powerless against files Git is already tracking. You must explicitly stop tracking them first.

```bash
# Step 1 — Add to .gitignore
echo "config.json" >> .gitignore

# Step 2 — Stop tracking (file stays on disk)
git rm --cached config.json

# Step 3 — Commit both changes together
git add .gitignore
git commit -m "chore: stop tracking config.json"
```

**For an entire folder:**

```bash
git rm --cached -r node_modules/
```

> ⚠️ `git rm --cached` removes from **future** commits only. The file still exists in commit history. If it had secrets, **rotate them immediately** — don't rely on history removal alone.

| Situation | Action |
|-----------|--------|
| Secret committed locally | `git reset` to undo, add to `.gitignore` |
| Secret committed and pushed | Rotate/revoke the secret immediately |
| Must scrub history | Use `git filter-repo` or BFG Repo-Cleaner |

---

<a id="working-with-remotes"></a>
## 🌐 Working with Remotes

### Push & Pull

```bash
git push origin master           # Push local commits to remote
git pull origin master           # Fetch + merge remote changes into your branch
git fetch origin                 # Download remote info WITHOUT merging
```

### Understanding Remote Names

| Term | Meaning |
|------|---------|
| `origin` | Nickname for your remote repo (set automatically on clone) |
| `master` / `main` | Your local branch name |
| `origin/master` | Git's local snapshot of GitHub's master branch |
| `origin/HEAD` | Pointer to the default branch on the remote |

> `git fetch` is like checking your mailbox — picks up letters but doesn't open them. `git pull` = `git fetch` + `git merge`.

---

<a id="help-documentation"></a>
## 🆘 Help & Documentation

```bash
git help <command>               # Full manual page (opens in pager — press 'q' to quit)
git <command> --help             # Same as above, different syntax
git <command> -h                 # Quick flag summary (stays in terminal)
```

> Use `-h` 90% of the time. Use `--help` when you need the full story.

---

<a id="git-internals"></a>
## 🔬 Git Internals

### The Object Model

Git stores everything as four object types inside `.git/objects/`:

| Object | What It Stores |
|--------|---------------|
| **Blob** | The content of a single file (no filename) |
| **Tree** | A directory listing (filenames → blobs and subtrees) |
| **Commit** | Points to a tree + author + message + parent commit |
| **Tag** | A named pointer to a specific commit |

### How They Connect

```
Commit ──> Tree (root)
            ├── blob (README.md)
            └── tree (basics/)
                 ├── blob (git-clone.md)
                 └── blob (git-help.md)
```

> A commit doesn't store files directly. It points to a tree, which points to blobs and subtrees.

### SHA-1 Hashes

Every Git object has a 40-character hexadecimal ID generated from its content.

```bash
echo "hello" | git hash-object --stdin    # Generate a SHA-1 hash
git fsck                                   # Verify integrity of all objects
```

**Properties:**
- Deterministic — same input → same hash
- Avalanche effect — change one byte → completely different hash
- Used for deduplication — identical content → stored only once

### The HEAD Pointer

HEAD answers "where am I right now?" In normal work, it points to a branch:

```
HEAD ──> master ──> a1b2c3d (latest commit)
```

```bash
cat .git/HEAD                    # See what HEAD points to
cat .git/refs/heads/master       # See what commit master points to
```

| Action | What Moves |
|--------|-----------|
| `git commit` | HEAD stays; the branch moves forward |
| `git checkout <branch>` | HEAD moves to the new branch |
| `git checkout <hash>` | HEAD points directly to a commit (**detached HEAD**) |

> ⚠️ In detached HEAD state, any commits you make are not on a branch. Switch away without creating a branch and those commits become orphaned and can be lost.

### Inspecting Objects

```bash
git cat-file -t <hash>           # Show object type (blob, tree, commit, tag)
git cat-file -p <hash>           # Show object content
```

---

<a id="commit-messages"></a>
## 💬 Commit Messages

### Anatomy of a Good Message

```
<type>: <short summary, 50 chars or less>

<optional longer description explaining WHY,
wrapped at around 72 characters>
```

### Common Types (Conventional Commits)

| Type | Use For |
|------|---------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `style` | Formatting, whitespace |
| `refactor` | Code restructuring, no behavior change |
| `test` | Adding or updating tests |
| `chore` | Maintenance, dependencies, config |
| `init` | Initial setup (first commit) |

### Golden Rules

- Use **imperative mood** — "add feature" not "added feature"
- Keep summary **under 50 characters**
- Explain the **WHY**, not the **WHAT**
- One logical change per commit

### The Imperative Mood Trick

> Complete the sentence: *"If applied, this commit will ___"*
> ✅ "If applied, this commit will **add dark mode**"
> ❌ "If applied, this commit will **added dark mode**"

### Good vs Bad

| ❌ Bad | ✅ Good |
|--------|---------|
| `update` | `fix: handle null user in login form` |
| `stuff` | `feat: add dark mode toggle to settings` |
| `fixed it` | `fix: prevent duplicate submissions on slow networks` |

---

<a id="troubleshooting"></a>
## 🚨 Troubleshooting

### Push Rejected — Non-Fast-Forward

**Error:**
```
! [rejected]   master -> master (non-fast-forward)
```

**Cause:** Your local branch is behind the remote — someone pushed commits you don't have.

**Diagnosis:**
```bash
git fetch origin
git log --oneline --all --graph
```

**Fix (the safe way):**
```bash
git pull origin master           # Pull and merge remote changes
git push origin master           # Now push
```

> ⚠️ **Don't** use `git push --force` to "fix" this — it overwrites the remote and can erase other people's work. If you absolutely must force-push (e.g. after a rebase), use `git push --force-with-lease` instead — it's safer.

---

<a id="quick-reference"></a>
## ⚡ Quick Reference

The 10 most important commands, in real-workflow order:

```bash
# 1. Set up your identity (once per machine)
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 2. Start or get a project
git init                         # New project
git clone <url>                  # Existing remote project

# 3. Check what's going on
git status

# 4. Stage your changes
git add <file>                   # Specific file
git add .                        # Everything in current dir

# 5. Review what you're about to commit
git diff --staged                # See exactly what will be committed

# 6. Commit
git commit -m "feat: describe what you did"

# 7. View history
git log --oneline

# 8. Sync with remote
git fetch origin                 # See what's new without merging
git pull origin master           # Pull + merge
git push origin master           # Send your commits to GitHub

# 9. Get help
git <command> -h
```

---

<a id="common-mistakes"></a>
## ⚠️ Common Mistakes

### 1. Editing a file after `git add`, then committing
You'll commit the **old version** that was staged, not your latest edits. Always `git add` again after editing.

### 2. Using `git push --force` to fix a rejected push
This overwrites the remote and can destroy commits you don't have. Always `git pull` first instead.

### 3. Vague commit messages like "update" or "fixed stuff"
Useless when reviewing history months later. Use Conventional Commits and explain the *why*.

### 4. Mixing unrelated changes into one commit
Bug fix + new feature + typo fix in one commit makes history hard to read and impossible to revert cleanly. One logical change per commit.

### 5. Committing files that shouldn't be tracked (e.g. `.env`, `node_modules`)
Use `.gitignore` to prevent it. If already committed, fix with `git rm --cached <file>`.

### 6. Confusing Git and GitHub
Git is the tool on your machine. GitHub is a hosting platform. You can use Git without GitHub.

### 7. Forgetting that the email in `git config` must match your GitHub email
Otherwise commits won't link to your profile or count toward your contribution graph.

### 8. Using `mv` or `rm` instead of `git mv` and `git rm`
Git sees regular `mv` as "delete + new untracked file." Use `git mv` and `git rm` to keep history clean.

### 9. Working in detached HEAD state without realizing it
If `git status` shows "HEAD detached at <hash>", any commits you make won't be on a branch and can be lost when you switch away. Create a branch first: `git checkout -b new-branch`.

### 10. Not pulling before pushing on a shared branch
Leads to the "non-fast-forward" rejection. Make `git pull` part of your routine before pushing.

### 11. Using `git commit -am` and expecting new files to be committed
`-am` only stages tracked files. Brand new files are skipped silently. For mixed changes, use `git add -A && git commit -m "msg"` or run `git status` first to see what will be included.

### 12. Committing without reviewing the diff first
`git status` tells you *which* files changed. `git diff --staged` tells you *what* changed inside them. Skip this step and you'll eventually commit a debug `console.log`, a hardcoded password, or an experimental change you forgot about.

### 13. Using `git reset --hard` on a shared branch
Reset rewrites history. If those commits were already pushed and others have them locally, you'll create a conflict mess for the whole team. Use `git revert` for anything that's been shared.

### 14. Using `git reset --hard` without checking what you're about to lose
`--hard` wipes all uncommitted changes — staged, unstaged, everything. Always run `git status` first. There's no undo (though `git reflog` can sometimes save you for committed work).

### 15. Running `git clean -fdx` without previewing first
This deletes every untracked file including gitignored ones — `node_modules/`, `.env` files with secrets, build outputs, IDE configs. Always run `git clean -n` (or `-nd`) first to preview what will be deleted.