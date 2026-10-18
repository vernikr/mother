# Handoff: agent-first methodology research

> **Handoff status:** ready for the next agent
>
> **Repository:** `vernikr/mother`
>
> **Branch:** `main` (the handoff was first added in `f58d0ce`; verify the
> current revision with `git log -1 --oneline`)
>
> **Updated:** 2026-10-18

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
  that Project exactly once, and verified its initial Project status is `Todo`.
- Incorporated the owner's follow-up direction from
  [`owner-direction-2026-10-18.md`](owner-direction-2026-10-18.md).

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
4. Project fields, statuses, views and automations have not been fully
   inspected on the owner's actual organization Project or approved.
5. Project boards for the owner's repositories were not audited in the prior
   research because the local GitHub auth session lacked `read:project`.
6. No mass migration of the 37 repositories has been authorized or performed.
7. No final policy for the methodology repository itself has been approved.

## Exact next action

Start by reading this handoff, [`owner-direction-2026-10-18.md`](owner-direction-2026-10-18.md),
[Issue #1](https://github.com/vernikr/mother/issues/1),
the [organization Project](https://github.com/orgs/vernikr/projects/1) and
[`01-structure-and-files.md`](01-structure-and-files.md). Then inspect the
Project's actual fields and views without changing the approved direction.
First research the newly stated open questions and draft a proposal for the
documentation hierarchy, Issue/artifact model and Project field schema. Keep
`state.md` deferred. Present the proposal for owner agreement before
implementing it or migrating any other repository.

Do not start a mass migration or create a final template before the owner
approves the proposed source-of-truth and Project lifecycle decisions.

## Checks and evidence

Working directory: repository root `/Users/vernikr/Downloads/projects/mother`.

| Check | Result |
|---|---|
| `git diff --check` | Passed before the previous research commit |
| Application tests | Not applicable; this repository contains documentation only |
| Lint/typecheck/build | Not applicable; no application toolchain is configured |
| Git status at handoff start | Clean: `main...origin/main` |
| Published revision | Handoff and Project setup are pushed to `origin/main`; verify the current revision with `git log -1 --oneline` |
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
