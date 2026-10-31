# AGENTS.md

- To manage native GitHub sub-issues, use `<!-- issue-parent: #N -->` or `<!-- issue-parent: none -->` in the child Issue body; when working in another repository, bootstrap it with `.github/workflows/issue-parent-sync.yml` listening to `issues: [opened, edited]`, granting `permissions: issues: write`, and calling `vernikr/mother/.github/workflows/issue-parent-sync.yml@v1`.

## Retro

Retro helps reduce recurring friction and deviations from the expected flow by capturing useful context and turning it into improvements to future agent behavior and project setup.

Treat user messages as Retro candidates when they signal confusion, friction, objection, an unexpected suggestion, an unexplained choice, a change of direction or unclear intent. Focus on meaningful deviations that may recur; routine exchanges do not automatically need a Retro.

For a candidate, review the relevant conversation and project context, then check existing `[Retro]` Issues to avoid duplicates. Create or update a `[Retro]` Issue in this repository. Write a brief, natural account of what the agent proposed and how the user responded. Keep observed facts distinct from possible explanations; do not invent motives or rush to a root cause. Start new Retros with `Status: Investigating`.

When taking on investigation or follow-up for an existing Retro, consult [RETRO.md](RETRO.md) for the status-specific workflow.
