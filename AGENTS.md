# AGENTS.md

- For native GitHub sub-issues, ensure this repository has `.github/workflows/issue-parent-sync.yml` with `issues: [opened, edited]`, `permissions: issues: write`, and a job that calls `vernikr/mother/.github/workflows/issue-parent-sync.yml@v1`; create the caller workflow if missing, then set `<!-- issue-parent: #N -->` on its own line in the child Issue body (use `<!-- issue-parent: none -->` to detach).
