# Handoff: agent-first methodology research

> **Handoff status:** ready for the next agent
>
> **Repository:** `vernikr/mother`
>
> **Branch:** `main` (the handoff was first added in `f58d0ce`; verify the
> current revision with `git log -1 --oneline`)
>
> **Updated:** 2026-10-22

## Goal

Develop, with the owner's approval at each substantial change, an evolving
methodology matrix for long-lived projects developed entirely by AI agents. The
matrix must cover repository structure, key files and formats, root
`AGENTS.md`, GitHub Projects, development and CI/CD tooling, and profiles for
different platforms, stacks and primary users.

The owner's operating model is unusual and must remain explicit: they direct
the work but do not inspect source code, diffs, commits, pull requests or test
results. They only occasionally inspect high-level documentation and can do
deep web research better than the agent. The methodology therefore needs short
human-facing summaries, strong agent legibility, durable state, automatic
verification and explicit escalation.

## Completed in this phase

- Captured the original brief and constraints in [`context.md`](context.md).
- Added GitHub Projects as a first-class part of the methodology context.
- Researched current conventions for AI-agent instruction files, long-running
  agent workflows, resumability, progressive disclosure, verification,
  sandboxing and repository maintenance.
- Researched GitHub Projects semantics, including Issues versus Project items,
  fields, views, automation, GraphQL/API, source-of-truth boundaries and
  agent-facing workflows.
- Inspected representative owner repositories:
  `research-workspace`, `size-report`, `biblio-mcp`, `browser-agent`,
  `browser-agent-mcp`, `gemini-mcp`, `freebuff-watchdog`,
  `figma-safe-resets`, and `alfa-ui-kit`.
- Published the first working report in
  [`01-structure-and-files.md`](01-structure-and-files.md).
- Added navigation links from the root [`README.md`](../../README.md).
- Created the private organization-level GitHub Project [Project methodology matrix](https://github.com/orgs/vernikr/projects/1), linked to `vernikr/mother`.
- Created [Issue #1](https://github.com/vernikr/mother/issues/1), added it to
  that Project exactly once; its current Project status is `In Progress`.
- Incorporated the owner's follow-up direction from
  [`owner-direction-2026-10-18.md`](owner-direction-2026-10-18.md).
- Inspected the actual organization Project, its current fields, view and
  built-in workflows through the GitHub API.
- Drafted the proposal in
  [`02-proposal-documentation-and-project.md`](02-proposal-documentation-and-project.md).
- Incorporated the owner's first review on 2026-10-21: root feature tree,
  `docs/research/`, test-local fixtures, Issue-owned blockers, the revised
  feedback inbox/archive/outbox model, business evidence, no separate Verify
  status, and a separate Blocked view.
- Started the Project pilot: renamed the table view to `All work`, added the
  `Queue` board and `Blocked` table views, changed Status to `Inbox`, `Ready`,
  `In Progress`, `Blocked`, `Done`, and moved Issue #1 to `In Progress`.
- Added the root conformance entry point [`MOTHER.md`](../../MOTHER.md).
- Created the guideline epic [Issue #2](https://github.com/vernikr/mother/issues/2)
  and sub-issues for the conformance entry point, LLM prompts, fork divergence,
  MCP/CLI integration and universal logging ([#3](https://github.com/vernikr/mother/issues/3),
  [#4](https://github.com/vernikr/mother/issues/4), [#5](https://github.com/vernikr/mother/issues/5),
  [#6](https://github.com/vernikr/mother/issues/6), [#7](https://github.com/vernikr/mother/issues/7)).
 - Created the organization-level `Business Value` Issue field as a number and
   added it to the methodology Project. Initial working values were assigned to
   Issues #1–#7 after triage.
 - Added new guideline work for packaging documents versus skills, shared GUI/TUI
   UI/UX, supported stacks and versions, and cross-repository conformance
   tracking ([#8](https://github.com/vernikr/mother/issues/8),
   [#9](https://github.com/vernikr/mother/issues/9),
   [#10](https://github.com/vernikr/mother/issues/10), and epic
   [#11](https://github.com/vernikr/mother/issues/11) with [#12](https://github.com/vernikr/mother/issues/12)
   and [#13](https://github.com/vernikr/mother/issues/13)).

 - Added the repository-health and upstream-cause epic [#14](https://github.com/vernikr/mother/issues/14)
   with separate work for fork root-cause attribution and upstream issue
   reporting ([#15](https://github.com/vernikr/mother/issues/15)), adaptive
   deterministic codebase-health tooling ([#16](https://github.com/vernikr/mother/issues/16)),
   adaptive CI/CD and development dependencies ([#17](https://github.com/vernikr/mother/issues/17))
   and refactoring prompts/evidence tracking ([#18](https://github.com/vernikr/mother/issues/18)).

## New owner direction

The owner clarified that the next phase should first explore and gradually
introduce `decisions/index.md` with numbered decision records, a feature tree
with FR-IDs, a research and primary-materials area, fixtures, blockers,
documentation indexes, reflection/friction inboxes, visual representations of
complex algorithms and a constants registry. `state.md` is explicitly deferred.

For GitHub Projects, the owner wants an adaptable Issue template, no `Risk`
field for now, a researched `Business Value (BV)` field, and no assumed
calendar fields. `Priority` and `Agent state` remain open questions. One
Project may cover multiple repositories. The owner also wants follow-up prompts
and unchosen suggestions to be retained, low documentation-maintenance cost,
minimal repetition, system-level fixes and direct human-sounding responses.

## Current working recommendations

These are **not yet approved as final methodology**:

1. Keep a short plain-Markdown root `AGENTS.md` as a map and operating
   contract, not an encyclopedia.
2. Keep durable knowledge in structured `docs/` files.
3. Use GitHub Issue as the semantic work item and GitHub Project as the
   operational queue/projection; do not maintain a second master backlog in
   `TODO.md`.
4. Defer `docs/state.md` until the owner explicitly brings it back into scope.
5. Create repository plans only for multi-session, multi-repository,
   architecture-heavy or high-risk work; link each plan to its Issue.
6. Put hard guarantees in CI, tests, hooks, permissions, sandbox and policy,
   not only in prose instructions.
7. Prefer a base profile plus project-specific extensions over one mandatory
   directory tree for every stack.
8. For fork bugs, identify the responsible repository or component before
   opening an upstream Issue; search existing Issues first and link the local
   fix and evidence back to the responsible repository.

The report distinguishes confirmed external facts from working recommendations
and lists source URLs. Do not silently turn recommendations into policy.

## Source-of-truth model currently proposed

| Concern | Proposed owner |
|---|---|
| Problem, scope and acceptance criteria | GitHub Issue |
| Parent/child work hierarchy | Parent Issue and sub-issues |
| Queue, operational status and project metadata | GitHub Project |
| Architecture and durable invariants | `docs/architecture.md` |
| Rationale and durable technical decisions | `docs/decisions.md` or `docs/decisions/` |
| Long-running execution plan | `docs/plans/active/` |
| Current handoff/navigation state | `HANDOFF.md` for now; `docs/state.md` deferred |
| Actual implementation and runtime behavior | Repository/runtime |
| Verification evidence | GitHub Actions Checks, artifacts and smoke/eval outputs |

Avoid duplicating the same status, priority, date, acceptance criteria or
backlog item across these systems. A stored state claim must have a handle and
a cheap command/query that can re-check it.

## Not completed / open decisions

1. The root `AGENTS.md` template has not been designed or approved.
2. The `docs/state.md` schema and update protocol have not been designed or
   approved.
3. The Issue → Project → agent run → verification → Done lifecycle is not yet
   formalized.
4. Project fields, statuses, views and automations have been inspected on the
   methodology Project; the first approved pilot changes and the organization-
   level Business Value field are applied.
5. Project boards for the owner's other repositories were not audited in the prior
   research because the local GitHub auth session lacked `read:project`.
6. No mass migration of the 37 repositories has been authorized or performed.
7. No final policy for the methodology repository itself has been approved.
8. The packaging contract for guidelines, UI/UX requirements, supported stack
   matrix and cross-repository conformance model are open research topics.
9. The upstream-cause workflow, adaptive CI/CD baseline, deterministic health
   tooling and refactoring methodology are open research topics tracked under
   epic #14.

## Exact next action

Read this handoff, [`owner-direction-2026-10-18.md`](owner-direction-2026-10-18.md),
[Issue #1](https://github.com/vernikr/mother/issues/1),
the [organization Project](https://github.com/orgs/vernikr/projects/1),
[`01-structure-and-files.md`](01-structure-and-files.md) and the new
[`02-proposal-documentation-and-project.md`](02-proposal-documentation-and-project.md).
Read the new [`MOTHER.md`](../../MOTHER.md), review guideline epic #2,
conformance epic #11 and repository-health epic #14 with their sub-issues, and
continue the detailed research one profile at a time. Keep `state.md` deferred
and do not migrate any other repository before the relevant profile is agreed
and piloted.

Do not start a mass migration or create a final template before the owner
approves the proposed source-of-truth and Project lifecycle decisions.

## Checks and evidence

Working directory: repository root `/Users/vernikr/Downloads/projects/mother`.

| Check | Result |
|---|---|
| `git diff --check` | Passed before the current handoff |
| Application tests | Not applicable; this repository contains documentation only |
| Lint/typecheck/build | Not applicable; no application toolchain is configured |
| Git status at handoff start | Clean: `main...origin/main` |
| Published revision | Verify the current revision with `git log -1 --oneline` |
| GitHub Project API access | Available after `gh auth refresh -h github.com -s read:project,project`; Project and Issue #1 were created and read back successfully |

The last confirmed repository state was clean and synchronized with
`origin/main`; verify the current revision with `git log -1 --oneline`.

## GitHub access note

Do not copy or store tokens in this repository. The active `gh` account now has
the `project` scope. If a future session loses that authorization, refresh the
existing account with:

```text
gh auth refresh -h github.com -s read:project,project
```

The next agent should run `gh auth status` and
`gh project list --owner vernikr-personal`, then verify all Project mutations by
reading the resulting item and field values back through the API.
