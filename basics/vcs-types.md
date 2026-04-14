# Types of Version Control Systems

Version control systems evolved through three generations: Local, Centralized, and Distributed — each solving the limitations of the previous one.

## The Three Types

- `Local VCS` — Tracks changes only on your own machine. No collaboration. If the machine dies, history is lost. (e.g. RCS)
- `Centralized VCS (CVCS)` — One central server holds the repo. Everyone connects to it. Single point of failure. (e.g. SVN, CVS)
- `Distributed VCS (DVCS)` — Every developer has a full copy of the repo and its entire history. No single point of failure. Works offline. (e.g. Git, Mercurial)

## Key Takeaway

> The jump from Centralized to Distributed is what made modern collaboration possible. Git is a DVCS — every clone is a full backup, and you can commit, branch, and view history without any network connection.

## When to Use This

> Whenever someone asks why Git is better than older systems, or why you can work offline with Git — the answer is that Git is a Distributed VCS.