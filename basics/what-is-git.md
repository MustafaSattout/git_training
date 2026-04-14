# What is Git & Why Git?

Git is a free, open-source, distributed version control system designed for speed, data integrity, and large-scale project support.

## How Git Stores Data

- Git stores **snapshots** of the entire project at each commit, not deltas (differences)
- If a file hasn't changed, Git stores a pointer to the previous version instead of duplicating it
- This makes reading history and switching branches extremely fast

## Why Git Won

- `Speed` — Almost every operation is local (commit, branch, diff, log), so it's nearly instant
- `Data integrity` — Every object is checksummed with SHA-1. Corruption is detected automatically
- `Distributed` — Every clone is a full backup. No single point of failure. Full offline capability
- `Cheap branching` — Creating a branch takes milliseconds, encouraging experimentation
- `Open source` — Free, no licensing costs, massive community and ecosystem
- `GitHub` — Added a collaboration layer (PRs, issues, CI/CD) that made Git the industry standard

## Git vs Older Systems

- Older tools like SVN use a centralized model with delta storage — slower, server-dependent, fragile
- Git uses a distributed model with snapshot storage — faster, offline-capable, resilient

## When to Use This

> Whenever you need to explain what Git is, how it differs from older VCS tools, or why it's the industry standard. The snapshot model and distributed architecture are the two core ideas that set Git apart.
