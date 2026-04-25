# Git Architecture

Git has two layers: the three working areas you interact with daily, and the object model that powers everything under the hood.

## Layer 1 — The Three Areas

Working Directory  ──git add──>  Staging Area  ──git commit──>  Repository
(your files)                   (index)                       (.git/objects)

- `Working Directory` — Where you edit files
- `Staging Area (Index)` — Where you prepare what goes into the next commit
- `Repository` — Permanent history stored inside `.git`

## Layer 2 — The Object Model

Git stores everything as four types of objects inside `.git/objects/`:

- `Blob` — The content of a single file (no filename, just raw content)
- `Tree` — A directory listing, mapping filenames to blobs and subtrees
- `Commit` — A snapshot pointing to a tree, plus author, message, and parent commit
- `Tag` — A named pointer to a specific commit (e.g. "v1.0")

## How They Connect

Commit ──> Tree (root)
├── blob (README.md)
└── tree (basics/)
├── blob (git-clone.md)
└── blob (git-help.md)

## Inspecting Objects

```bash
git cat-file -t <hash>    # Show the type of an object (blob, tree, commit, tag)
git cat-file -p <hash>    # Show the content of an object
```

## Key Takeaway

> A commit doesn't store files directly. It points to a tree, which points to blobs and subtrees. If a file doesn't change between commits, Git reuses the same blob — making it fast and space-efficient.

## When to Use This

> Understanding the object model helps you troubleshoot, understand how branching works, and appreciate why Git is so efficient. It's the foundation everything else builds on.