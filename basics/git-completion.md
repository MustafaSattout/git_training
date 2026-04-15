# Git Completion

Tab auto-completion for Git commands, branch names, remotes, flags, and more — saves time and prevents typos.

## How to Use

- Type a partial command or name and press `Tab` to autocomplete
- `git che` + Tab → `git checkout`
- `git branch my-fea` + Tab → `git branch my-feature-branch`

## Status by OS

- `Mac (Zsh)` — Built-in since macOS Catalina. Works out of the box.
- `Windows (Git Bash)` — Built-in. Works out of the box.
- `Ubuntu/Linux (Bash)` — Usually included with Git install. If not, enable manually.

## Manual Setup (Linux/Bash)

```bash
# Check if completion script exists
ls /usr/share/bash-completion/completions/git

# Add to ~/.bashrc if not loading automatically
source /usr/share/bash-completion/completions/git

# Reload shell
source ~/.bashrc
```

## What It Autocompletes

- Commands (`commit`, `checkout`, `merge`, etc.)
- Branch names
- Remote names
- Flags (`--amend`, `--force`, etc.)
- Tag names

## When to Use This

> Always. Once it's enabled, use Tab constantly. It's faster and eliminates typos, especially with long branch names.
