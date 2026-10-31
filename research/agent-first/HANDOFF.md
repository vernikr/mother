# Handoff: agent-first methodology research

> **Status:** ready for the next agent
>
> **Repository:** vernikr/mother
>
> **Updated:** 2026-10-31

## Goal

Develop an owner-approved methodology matrix for long-lived projects developed
by AI agents. It covers repository structure, documentation, GitHub Projects,
CI/CD, health, refactoring and profiles for different stacks and users.

The owner directs work but does not inspect source code, diffs, commits, pull
requests or test results directly. The methodology therefore needs durable
knowledge, automatic verification, concise human-facing summaries and explicit
escalation.

## Read in this order

1. [MOTHER.md](../../MOTHER.md) — operational conformance guide;
2. [decisions/index.md](../../decisions/index.md) — working decisions and open
   policy questions;
3. [README.md](../../README.md) — repository navigation;
4. [02-proposal-documentation-and-project.md](02-proposal-documentation-and-project.md)
   — current design proposal;
5. [owner-direction-2026-10-18.md](owner-direction-2026-10-18.md) — owner
   constraints and unresolved intent;
6. [01-structure-and-files.md](01-structure-and-files.md) — historical first
   research report.

When sources disagree, use explicit owner-approved decisions first, then
MOTHER.md, working decisions, the current proposal, owner direction, research
and finally this handoff. This file records execution state; it does not change
methodology policy.

## Current repository state

- Branch: main.
- Current revision: verify with git log -1 --oneline.
- Application toolchain: not applicable; this repository contains documentation
  and methodology artifacts.
- state.md: deferred.
- Historical research remains under research/agent-first/; new research is
  intended for docs/research/ after that structure is approved.
- Durable decisions belong in top-level decisions/.

## Current Project pilot

Organization Project: [Project methodology matrix](https://github.com/orgs/vernikr/projects/1).

- statuses: Inbox, Ready, In Progress, Blocked, Done;
- views: All work, Queue, Blocked;
- numeric organization-level Business Value, 0–5;
- built-in Repository field for cross-repository scope;
- no separate Verify, Risk, calendar, Priority or Agent state baseline.

Issue #1 is the active methodology work item. Current epics:

- #2 — reusable project guidelines;
- #11 — cross-repository conformance tracking;
- #14 — repository health and upstream-cause maintenance;
- #19 — owner-facing project controllability and information representations.

## Native GitHub sub-issues

[Decision 0005](../../decisions/0005-native-github-sub-issues.md) is approved.
[Issue #24](https://github.com/vernikr/mother/issues/24) records the problem,
options, selected design and completed implementation. The reusable workflow
and canonical caller template are stored in mother; see
[the retrospective](03-github-sub-issue-workflow.md).

A target repository needs a small local caller at
.github/workflows/issue-parent-sync.yml. If it is missing, copy the
canonical template verbatim from the URL documented in MOTHER.md.
Use the standalone set/move marker with a parent number, or the standalone
`<!-- issue-parent: none -->` marker to detach. See MOTHER.md for exact syntax.
Verify both the Actions run and the resulting native parent link.

## Working principles

- Keep a small base profile and add project-specific extensions.
- Use GitHub Issues for semantic work and the Project for operational status.
- Do not create a second master backlog in TODO.md.
- For fork bugs, identify the responsible repository or component, search its
  existing Issues, then create or reuse an upstream report with evidence.
- Treat FEATURES.md and ALGORITHMS.md as examples of a broader owner-facing
  information system.
- Keep canonical text/data separate from generated diagrams, images, dashboards
  and presentations.
- Capture a baseline before health work or refactoring and report actual checks,
  measured results and residual risk.

## Open work

1. Resolve the artifact lifecycle question in
   [decisions/0004-artifact-lifecycle.md](../../decisions/0004-artifact-lifecycle.md).
2. Formalize the Issue → Project → evidence → Done lifecycle and enforcement.
3. Research and pilot the guideline epics one profile at a time.
4. Decide the universal owner-facing information baseline and adaptive
   representation stack.
5. Add lightweight consistency checks after canonical paths and statuses are
   stable.

Do not start a mass migration or create a final universal template before the
relevant profile and Project lifecycle are approved and piloted.

## Latest checks

| Check | Result |
|---|---|
| git diff --check | Run when the documentation branch is prepared for merge |
| Markdown relative links | Verify all changed links before merge |
| Application tests/build | Not applicable; no application toolchain |
| GitHub Project readback | Project data should be reconciled for each completed work item |
| Git status | Verify with git status --short --branch |

If Project access is unavailable, refresh the existing GitHub account with:

    gh auth refresh -h github.com -s read:project,project
