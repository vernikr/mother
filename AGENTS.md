# AGENTS.md

- To manage native GitHub sub-issues, use `<!-- issue-parent: #N -->` or `<!-- issue-parent: none -->` in the child Issue body; when working in another repository, bootstrap it with `.github/workflows/issue-parent-sync.yml` listening to `issues: [opened, edited]`, granting `permissions: issues: write`, and calling `vernikr/mother/.github/workflows/issue-parent-sync.yml@v1`.
