# Git Help

Git has built-in documentation for every command, accessible directly from the terminal.

## Three Ways to Get Help

```bash
git help <command>       # Full manual page (detailed, opens in pager — press 'q' to quit)
git <command> --help     # Same full manual page, different syntax
git <command> -h         # Quick summary of flags and usage (stays in terminal)
```

## When to Use Which

- `git help <command>` or `--help` — When you want to deeply understand a command
- `git <command> -h` — Quick reminder of available options. Use this 90% of the time

## Examples

```bash
git help config          # Full docs on git config
git branch --help        # Full docs on git branch
git log -h               # Quick flag summary for git log
```

## When to Use This

> Whenever you forget a flag or want to understand what a command can do. Try `-h` first for a quick answer. Use `--help` when you need the full story.
