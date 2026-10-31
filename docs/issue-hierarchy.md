# Native GitHub issue hierarchy

## Contract

Put one standalone HTML comment in the child Issue body:

- `<!-- issue-parent: #123 -->` — add the Issue under parent #123 or move it there.
- `<!-- issue-parent: none -->` — remove the current parent.

After a successful operation the workflow removes the command from the Issue body. Parent and child must be Issues in the same repository. A pull request cannot be a parent. The current central workflow only processes changes made by the `vernikr-personal` GitHub account.

## Repository bootstrap

A repository with Issues enabled needs a small caller workflow at `.github/workflows/issue-parent-sync.yml`:

```yaml
name: Issue hierarchy

on:
  issues:
    types: [opened, edited]

permissions:
  issues: write

jobs:
  sync-parent:
    uses: vernikr/mother/.github/workflows/issue-parent-sync.yml@v1
```

The caller delegates all implementation to the reusable workflow in this repository. No PAT or repository secret is needed; GitHub supplies the caller repository's `GITHUB_TOKEN`.

After adding the file, verify that Actions are enabled and that a run named **Issue hierarchy** succeeds. Public caller repositories can use GitHub's free standard hosted runners. Private caller repositories still need usable Actions quota/billing or a self-hosted runner; sharing a reusable workflow does not move runner usage to this central repository.

## New repositories

When an agent needs to create or use sub-issues in a repository, it should first check for the caller workflow above. If it is missing, add it before writing the command marker. If Actions are unavailable, report that blocker rather than leaving an unprocessed command in the Issue body.

Keep the one-line bootstrap rule in the repository's `AGENTS.md` so future agents can self-configure.
