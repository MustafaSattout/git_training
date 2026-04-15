# Push Rejected — Non-Fast-Forward Error

When you try to push and Git says "rejected — non-fast-forward", it means the remote has commits your local branch doesn't have. Git refuses to push because it would overwrite those commits.

## The Error

```
$ git push origin master
To github-personal:username/repo.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'github-personal:username/repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
```

## Why This Happens

Your local branch is **behind** the remote. Someone (or you from another machine, or a direct GitHub edit) pushed commits that your local copy doesn't have yet.

```
Your local:     A — B — C              (master)

Remote:         A — B — C — D — E      (origin/master)
                             ↑   ↑
                     these commits exist on GitHub
                     but NOT on your local machine
```

You're trying to push `C` as the latest, but GitHub already has `D` and `E` after `C`. Git refuses because your push would erase `D` and `E`.

## How to Diagnose

### Step 1 — Fetch the latest remote info

```bash
git fetch origin
```

- Goes to GitHub and downloads new commits, branches, and tags
- Does NOT change your working files or local branches
- Think of it as "checking your mailbox without opening the letters"

### Step 2 — Visualize the difference

```bash
git log --oneline --all --graph
```

| Flag        | What It Does                                                  |
|-------------|---------------------------------------------------------------|
| `--oneline` | One commit per line (short hash + message)                    |
| `--all`     | Shows all branches — local and remote                         |
| `--graph`   | Draws ASCII lines showing how branches connect and diverge    |

Example output:

```
* d4e5f6a (origin/master) some commit made on GitHub
* c3b2a1f (HEAD -> master) docs: add notes on Git completion
* a1b2c3d docs: add notes on Git installation
```

This tells you: `origin/master` is one commit ahead of your local `master`.

## How to Solve

### The Safe Way — Pull Then Push

```bash
# 1. Pull remote changes and merge into your local branch
git pull origin master

# 2. Now push — your local has everything the remote has, plus your work
git push origin master
```

`git pull` = `git fetch` + `git merge` combined into one command. It downloads the remote changes and merges them into your current branch.

If there are no conflicts, Git merges automatically and you can push right away.

### If You Get a Merge Conflict After Pulling

Don't panic. It means you and the remote changed the **same lines** in the **same file**. Git doesn't know which version to keep, so it asks you to decide.

```bash
# 1. Open the conflicting file — look for conflict markers
<<<<<<< HEAD
your local changes
=======
remote changes
>>>>>>> origin/master

# 2. Edit the file — keep what you want, remove the markers

# 3. Stage the resolved file
git add <filename>

# 4. Complete the merge
git commit -m "merge: resolve conflict with remote"

# 5. Push
git push origin master
```

## Key Terms Explained

| Term             | What It Means                                                              |
|------------------|----------------------------------------------------------------------------|
| `origin`         | Nickname for your remote repository (usually GitHub)                       |
| `master`         | Your local branch                                                          |
| `origin/master`  | Git's local snapshot of GitHub's `master` branch (updated on fetch/pull)   |
| `origin/HEAD`    | Pointer to the default branch on GitHub (usually `origin/master`)          |
| `fast-forward`   | A clean merge where Git just moves the pointer forward — no divergence     |
| `non-fast-forward` | The branches have diverged — Git can't just move the pointer, it needs a merge |

## What NOT to Do

```bash
# DON'T do this unless you truly understand the consequences
git push --force origin master
```

`--force` overwrites the remote history with your local version. This **permanently erases** commits on the remote that you don't have locally. In a team environment, this can destroy other people's work.

The only time `--force` is acceptable is when you've intentionally rewritten history (e.g., after `git rebase`) and you know exactly what you're overwriting. Even then, prefer `--force-with-lease` which is safer:

```bash
# Safer alternative — only force-pushes if no one else pushed since your last fetch
git push --force-with-lease origin master
```

## Quick Fix Cheatsheet

```bash
# Diagnose
git fetch origin
git log --oneline --all --graph

# Fix (safe way)
git pull origin master
git push origin master

# If merge conflict after pull
# → edit conflicting files → remove markers → git add → git commit → git push
```

## When to Use This

> Whenever you see "rejected — non-fast-forward" after a push. The cause is always the same: your local branch is behind the remote. The fix is always: pull first, resolve any conflicts, then push.
