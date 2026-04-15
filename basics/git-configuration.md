# Git Configuration

Set your identity and preferences before using Git. This tells Git who you are and how you like to work.

## Three Configuration Levels

- `--system` — Applies to every user on the machine. Stored in `/etc/gitconfig`
- `--global` — Applies to you, across all repos. Stored in `~/.gitconfig`
- `--local` — Applies to one specific repo only. Stored in `.git/config` inside the repo
- Priority: Local > Global > System

## Essential Setup

```bash
git config --global user.name "Your Name"       # Your name on every commit
git config --global user.email "your@email.com"  # Must match your GitHub email
```

## Useful Settings

```bash
git config --global init.defaultBranch main      # Default branch name for new repos
git config --global core.editor "code --wait"    # Set VS Code as your editor
```

## Viewing Configuration

```bash
git config --list       # See all current settings
git config user.name    # Check a specific setting
```

## When to Use This

> Run the essential setup once on any new machine. Use `--local` when you need a different identity for a specific repo (e.g. work vs personal email).