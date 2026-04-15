# The Git Landscape

Git is the version control engine. GitHub, Bitbucket, and Stash are cloud platforms that host Git repos and add collaboration tools on top.

## The Key Distinction

- `Git` — The version control system itself. Runs locally in your terminal. Handles commits, branches, merges, history.
- `Hosting platforms` — Store your Git repos in the cloud and add features like pull requests, issue tracking, CI/CD, and team management.

## The Major Platforms

- `GitHub` — Most popular. Largest open-source community. Features: Pull Requests, Issues, Actions (CI/CD), Copilot. Used by most developers and open-source projects.
- `Bitbucket` — Atlassian's cloud platform. Tight integration with Jira and Confluence. Features: Pipelines (CI/CD), free private repos. Popular with Atlassian-based teams.
- `Stash (Bitbucket Server / Data Center)` — Atlassian's self-hosted version of Bitbucket. Same features but runs on the company's own servers. Used by enterprises that need full control over infrastructure. "Stash" is the old name — it was rebranded to Bitbucket Server, then Bitbucket Data Center.

## Key Takeaway

> Git commands are universal across all platforms. `git commit`, `git push`, `git branch` work exactly the same whether your remote is on GitHub, Bitbucket, or a self-hosted server. The platform only changes where your repo is hosted and what web-based tools you get.

## When to Use This

> Whenever you need to explain the difference between Git and the platforms built around it, or when comparing hosting options for a team or company.
