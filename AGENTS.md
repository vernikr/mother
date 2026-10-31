# AGENTS.md

- To manage native GitHub sub-issues, use `<!-- issue-parent: #N -->` or `<!-- issue-parent: none -->` in the child Issue body; when working in another repository, bootstrap it with `.github/workflows/issue-parent-sync.yml` listening to `issues: [opened, edited]`, granting `permissions: issues: write`, and calling `vernikr/mother/.github/workflows/issue-parent-sync.yml@v1`.

## Working conventions

- Use English for repository artifacts and Russian for chat replies.
- Treat context-window efficiency as the top priority: keep code, instructions and documentation compact; avoid duplication and details recoverable from source files.
- Write comments only for non-obvious behavior or invariants. Omit historical rationale and details evident from the code.
- Keep evergreen documentation free of volatile numeric facts; update relevant evergreen docs after functional changes.
- Use Conventional Commits: `type(scope): description`. For versioned software, use SemVer: fixes → PATCH, features → MINOR, breaking changes → MAJOR.
- When a task needs a durable plan or report, use `docs/worklog/YYYY.MM.DD-task-slug/`; move completed task folders to `docs/worklog/archive/`. GitHub Issues and Projects remain the source of truth for work status.
- Before changing any tracked repository content or configuration, find an open GitHub Issue that clearly covers the work or create one. This applies to code, documentation, tests, configuration, workflows and other repository files, including small changes.
- Keep one Issue for each coherent change; split independent work when appropriate. Reference the Issue in the commit or pull request, then update it with the result and verification evidence. Read-only inspection does not require an Issue.
- If the repository's Issue tracker or required permissions are unavailable, do not make repository changes; report the blocker.
- When recurring friction, unclear expectations, repeated corrections or avoidable steps suggest that agent guidance could be improved, proactively propose a focused update to the relevant `AGENTS.md`. State the observed gap and expected benefit, and suggest concise wording. Prefer reusable guidance over one-off rules.
- In chat, lead with the outcome and write for a busy, non-technical owner: plain business language, concise, with decisions or required actions up front. Avoid code-level details and unexplained jargon.

## ChatGPT Web Free

- Use only capabilities exposed in the current session. Do not assume access to the user's machine, a local terminal or filesystem, installed tools, unconnected MCP servers, or persistent sandbox state. Verify that a required capability is available before relying on it; report the specific blocker when it is not.
- Work autonomously within the task and available permissions. Publish, deploy and update assets across reachable environments without seeking redundant approval; verify the result and report any environment the session could not reach.

## Retro

Retro helps reduce recurring friction and deviations from the expected flow by capturing useful context and turning it into improvements to future agent behavior and project setup.

Treat user messages as Retro candidates when they signal confusion, friction, objection, an unexpected suggestion, an unexplained choice, a change of direction or unclear intent. Focus on meaningful deviations that may recur; routine exchanges do not automatically need a Retro.

For a candidate, review the relevant conversation and project context, then check existing `[Retro]` Issues to avoid duplicates. Create or update a `[Retro]` Issue in this repository. Write a brief, natural account of what the agent proposed and how the user responded. Keep observed facts distinct from possible explanations; do not invent motives or rush to a root cause. Start new Retros with `Status: Investigating`.

When taking on investigation or follow-up for an existing Retro, consult [RETRO.md](RETRO.md) for the status-specific workflow.
