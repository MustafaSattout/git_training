# A Note on Commit Messages

A good commit message explains WHAT changed and WHY, making your project history readable and searchable for your future self and your team.

## Anatomy of a Good Commit Message

<type>: <short summary, 50 chars or less>
<optional longer description explaining WHY,
wrapped at around 72 characters>

## Common Types (Conventional Commits)

- `feat` — A new feature
- `fix` — A bug fix
- `docs` — Documentation only
- `style` — Formatting, whitespace, no logic changes
- `refactor` — Code restructuring, no behavior change
- `test` — Adding or updating tests
- `chore` — Maintenance, dependencies, build config
- `init` — Initial setup (first commit)

## Golden Rules

- Use imperative mood — "add feature" not "added feature"
- Keep the summary under 50 characters
- Explain the WHY, not the WHAT — the code shows what changed
- One logical change per commit — don't mix unrelated things

## The Imperative Mood Trick

> Complete the sentence "If applied, this commit will ___". If your message fits, it's correct.
> ✅ "If applied, this commit will **add dark mode**"
> ❌ "If applied, this commit will **added dark mode**"

## Good vs Bad Examples

- ❌ `update` → ✅ `fix: handle null user in login form`
- ❌ `stuff` → ✅ `feat: add dark mode toggle to settings`
- ❌ `fixed it` → ✅ `fix: prevent duplicate submissions on slow networks`

## Commands

```bash
git commit -m "fix: handle null user in login form"         # Short message
git commit                                                  # Opens editor for long message
git commit -m "summary" -m "longer description here"        # Summary + details
```

## When to Use This

> Every time you commit. A few seconds of thought on the message saves hours of confusion later when you or a teammate needs to understand the history.