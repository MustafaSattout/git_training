# Git Clone

`git clone` downloads a complete copy of a remote repository to your local machine, including all files, branches, and history.

## The Command

```bash
git clone <url>                      # Clone into a folder named after the repo
git clone <url> my-folder            # Clone into a custom folder name
git clone git@github.com:user/repo.git   # Clone using SSH
```

## What You Get

- All files from the remote repo
- All branches and tags
- The complete commit history
- The `origin` remote automatically configured — ready for `git pull` and `git push`

## Clone vs Init

- `git init` — Start a brand new project from scratch on your machine
- `git clone` — Get a copy of an existing repo from GitHub (or any remote)

## Key Detail

> After cloning, you don't need to manually set up the remote — `origin` is already configured. You can immediately pull, push, and collaborate.

## When to Use This

> Whenever you want to work on an existing remote repo — contributing to open source, joining a team project, or getting your own repo onto a new machine.
