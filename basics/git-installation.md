# Git Installation

Install Git on your machine so the `git` command is available in your terminal.

## Installation by OS

### Ubuntu/Linux
```bash
sudo apt update          # Update package list
sudo apt install git     # Install Git
```

### Mac
```bash
xcode-select --install   # Option 1: Xcode Command Line Tools (simplest)
brew install git          # Option 2: Via Homebrew
```

### Windows 11
- Download installer from https://git-scm.com/download/win
- Run the installer — this gives you Git Bash (a Linux-style terminal for Git)

## Verify Installation
```bash
git --version            # Should print something like: git version 2.x.x
```

## When to Use This

> First thing to do on any new machine or fresh OS install. If `git --version` doesn't work, Git isn't installed yet.