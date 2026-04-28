# SHA-1 Hash Values

Git uses SHA-1 to generate a unique 40-character hexadecimal ID for every object — blobs, trees, commits, and tags. This is how Git identifies, deduplicates, and integrity-checks everything.

## Key Properties

- `Deterministic` — Same input always produces the same hash
- `Unique` — Different input produces a completely different hash
- `One-way` — Cannot reverse-engineer content from the hash
- `Avalanche effect` — Change one character → entire hash changes

## Commands

```bash
echo "hello" | git hash-object --stdin    # Generate SHA-1 hash of a string
git log -1 --format="%H"                  # Full 40-char hash of latest commit
git log -1 --format="%h"                  # Short 7-char hash of latest commit
git log --oneline                         # Commit history with short hashes
git fsck                                  # Verify integrity of all objects in the repo
```

## Why Hashes Matter

- `Data integrity` — Git checksums every object. If content is corrupted, the hash won't match and Git detects it automatically
- `Unique identification` — Every commit, file version, and tree has a guaranteed unique ID
- `Deduplication` — Identical content produces the same hash, so Git stores it only once

## Short vs Full Hash

- Full hash: `e83c5163316f89bfbde7d9ab23ca2e25604af290` (40 characters)
- Short hash: `e83c516` (first 7 characters — usable as long as it's unique in the repo)

## When to Use This

> Understanding hashes helps you read `git log` output, reference specific commits, and trust that Git's integrity checking protects your data.