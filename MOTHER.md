# Mother conformance guide

This file is the short entry point for an agent asked to bring a project into
alignment with the methodology described by vernikr/mother.

For the methodology repository itself, use [decisions/index.md](decisions/index.md)
to resolve current working decisions and open questions. Research and proposals
provide evidence and design options; they do not silently override operational
guidance.

## 1. Start with the project, not with the template

Read the target repository's README, instruction files, current handoff/state,
tests and documentation map. Identify its platform, stack, primary user,
external systems and current source-of-truth boundaries.

Then read the relevant parts of the current mother methodology. Do not assume
that every optional directory or profile applies to every project.

## 2. Inspect before changing

Compare the project with the baseline and select only applicable profiles:

- documentation and agent instructions;
- product functionality tree;
- LLM and prompt registry;
- fork divergence registry;
- MCP and CLI skill integration;
- universal logging requirements;
- GUI and TUI UI/UX requirements;
- approved stack, framework and package-manager versions;
- repository health and CI/CD/development-dependency requirements;
- deterministic codebase-health checks and refactoring evidence;
- owner-facing project understanding and controllability;
- platform, UI, service, research or unattended-operation extensions.

For a bug in a fork, also map the fork lineage and relevant component boundaries
before editing. Decide whether the original cause is in the fork, its immediate
upstream, an upstream ancestor or an external component. Record the evidence and
the candidate responsible repository.

Record missing pieces, contradictions and obsolete documents before editing.

Guidelines may eventually be delivered as repository documents, agent skills,
or a deliberate combination of both. Until that contract is approved, do not
duplicate a guideline in two places: identify the canonical source and treat
any skill as an operational adapter around it.

## 3. Work from an Issue

Create or use one GitHub Issue for the conformance work. Split substantial
independent profiles into sub-issues. Use a repository plan only when the work
spans sessions, repositories, external systems or difficult verification.

Do not create a parallel master backlog in TODO.md. Keep the Issue and Project
as the work-management source of truth and keep durable project knowledge in
the repository.

### Native GitHub sub-issues

Use GitHub's native parent/sub-issue relationship. Add exactly one standalone
command line to the child Issue body:

- <!-- issue-parent: #123 --> — set or move the child under Issue #123;
- <!-- issue-parent: none --> — detach the child from its current parent.

The command is processed when the Issue is opened or edited, then removed after
the hierarchy operation succeeds. Parent and child must be Issues in the same
repository. A pull request cannot be a parent. After submitting the command,
verify that the Actions run succeeded and that GitHub shows the expected parent.

If the target repository lacks
.github/workflows/issue-parent-sync.yml, fetch the canonical file from
https://raw.githubusercontent.com/vernikr/mother/main/.github/workflow-templates/issue-parent-sync.yml
and copy it verbatim to that path. Never recreate the caller YAML manually.
It delegates to the reusable implementation in
vernikr/mother/.github/workflows/issue-parent-sync.yml@v1.

The local caller is required for GitHub to receive Issue events in that
repository; the implementation and installation template remain centralized
in mother. Routine agent instructions stay in the owner's global AGENTS.md;
do not replicate them across repository-level instruction files. See
[Decision 0005](decisions/0005-native-github-sub-issues.md) and the
[implementation retrospective](research/agent-first/03-github-sub-issue-workflow.md).

When a fork bug is caused outside the owned fork, search the candidate responsible
repository for similar open, closed and duplicate Issues before creating one. If
no suitable Issue exists, create the report from the owner's account in that
repository after implementing and verifying the necessary local fix. Include the
reproduction, root-cause evidence, fork lineage, component boundary, local fix or
workaround, and links to relevant commits or pull requests. Re-read the created
or reused Issue and record its URL and state in the local work item.

## 4. Apply the smallest complete change

Prefer a short root map and focused documents over a large universal template.
Add a file only when the project has the corresponding need. Preserve existing
useful documentation by merging or linking it instead of creating duplicates.

For human-review documents longer than two or three pages, add a clickable
Markdown table of contents. Detailed low-level documents intended mainly for
agents do not need one.

## 5. Verify and reconcile

Run the project's real checks. Verify documentation links, required structure,
runtime behavior and any profile-specific contracts. Record actual evidence,
business-facing measurements and known gaps. Re-read changed GitHub Issue and
Project items after updates.

Do not mark work complete because a document or command exists. Mark it complete
only when its acceptance criteria and evidence are satisfied, or explicitly
report what remains blocked.

For refactoring or health work, capture a baseline before changing code and
report the intended invariant, deterministic checks, measured result and residual
risk. Do not treat a health score or static finding as proof of a defect without
project-specific evidence.

For owner-facing project understanding, distinguish universal information from
profile-specific representations. Keep canonical facts in maintainable text or
structured data, and generate diagrams, images, dashboards or presentations when
they reduce the owner's reading cost. The agent should justify the selected
representation stack, avoid duplicate sources of truth, record freshness checks,
and report what remains uncertain.

## 6. Stop and ask the owner when

- the target project needs a new methodology rule rather than an application of
  an existing one;
- two documents or systems claim the same source of truth;
- migration would remove information or change a public contract;
- the applicable profile is unclear;
- required external access or a destructive action is unavailable.

Finish with a short human-facing report: changed files, selected profiles,
checks and evidence, remaining gaps, and the next recommended step.
