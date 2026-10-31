# Native GitHub sub-issue workflow

> **Status:** implemented; retrospective and operating contract  
> **Date:** 2026-10-31  
> **Work item:** [Issue #24](https://github.com/vernikr/mother/issues/24), child of [Issue #13](https://github.com/vernikr/mother/issues/13)  
> **Decision:** [0005 — Native GitHub sub-issues](../../decisions/0005-native-github-sub-issues.md)

## Problem

Agents need to manage parent/child relationships between GitHub Issues as part of ordinary project work. The hierarchy must be native to GitHub so that it remains visible in the issue itself and can be used by GitHub Projects.

A chat instruction alone is insufficient: an agent needs a deterministic operation that can be submitted through the tools it already uses to edit an Issue. Manual UI operations are awkward for unattended agent work. Asking every agent to compose workflow YAML adds a fragile authoring step; copying the full implementation into every repository creates multiple sources that can drift.

## Requirements

1. Create, change and remove native GitHub Issue parent relationships.
2. Express the requested operation through one explicit line in the child Issue body.
3. Process the command automatically when an Issue is opened or edited.
4. Keep executable logic in one central repository.
5. Make installation in another repository a deterministic copy operation.
6. Keep routine agent instructions in the owner's global AGENTS.md, without replicating them across project repositories.
7. Verify the resulting GitHub relationship instead of treating a successful edit as proof.

## Proposals considered

| Proposal | Benefit | Cost or limitation |
|---|---|---|
| Manage the hierarchy manually in GitHub UI | No automation to maintain | Awkward for agents and unattended workflows |
| Copy the full implementation into each repository | Self-contained local setup | Duplicated logic; updates and fixes must be propagated |
| Keep a reusable workflow in mother and let agents author a small local caller | One implementation, small local file | Agents can make mistakes while writing YAML |
| Keep the reusable workflow and a canonical caller template in mother | One implementation; exact local setup can be copied | Each target repository still needs its own event-triggering caller |

## Selected solution

Use a central reusable workflow and a canonical copyable caller template.

### Canonical files

- **Reusable implementation:** .github/workflows/issue-parent-sync.yml
- **Caller template:** .github/workflow-templates/issue-parent-sync.yml

The implementation contains the shared logic and exposes workflow_call. In every target repository that needs this feature, the small caller is copied verbatim to:

    .github/workflows/issue-parent-sync.yml

The caller subscribes to issues.opened and issues.edited, grants issues: write, and invokes:

    vernikr/mother/.github/workflows/issue-parent-sync.yml@v1

The local caller is necessary because GitHub must receive the Issue event in the repository where the Issue lives. It is only an event adapter; it contains no parent-management logic.

The v1 tag is the shared release reference. Updates to the reusable implementation must be published to that tag so existing callers receive the new implementation on subsequent runs.

### Agent installation rule

When the target repository lacks .github/workflows/issue-parent-sync.yml, fetch:

    https://raw.githubusercontent.com/vernikr/mother/main/.github/workflow-templates/issue-parent-sync.yml

Copy the response verbatim to the target path. Do not recreate or rewrite the YAML manually. The template is canonical; local customizations would create drift.

Agent operating instructions remain in the owner's global AGENTS.md. The presence of a local workflow file is a technical GitHub requirement, not a reason to duplicate instruction files across repositories.

## Command contract

Place exactly one standalone marker in the child Issue body:

    <!-- issue-parent: #123 -->

Sets the parent to Issue #123. If a different parent already exists, the workflow moves the child to the requested parent.

    <!-- issue-parent: none -->

Removes the current parent.

The workflow runs on Issue creation or edit. After a successful hierarchy operation, it removes the one-shot marker from the body. It validates that the child and parent are Issues in the same repository, rejects self-parenting, and does not use pull requests as parents. The current implementation accepts commands only when the event sender is vernikr-personal.

After issuing the command, the agent must verify both the Actions run and the native parent relationship. A marker disappearing or an Issue edit succeeding alone is insufficient evidence.

## Implementation record

- The reusable implementation is present in .github/workflows/issue-parent-sync.yml.
- The copyable caller is present in .github/workflow-templates/issue-parent-sync.yml.
- The template was read back after creation and checked against the intended caller.
- The v1 tag resolves to the reusable implementation.
- [Issue #24](https://github.com/vernikr/mother/issues/24) preserves the problem statement, proposals, selected design, requirements and completion record.
- The work is tracked under the Project-structure epic [Issue #13](https://github.com/vernikr/mother/issues/13).

## Operational constraints and follow-ups

- **Runner billing belongs to the caller repository.** Reusable workflows do not transfer GitHub Actions runner consumption to mother. A billing or spending-limit block must be resolved for the repository whose event triggered the run.
- **Authorization is owner-specific.** The current implementation ignores commands from senders other than vernikr-personal. Broader authorization requires an explicit decision and a code change.
- **The template is a snapshot.** Existing caller files do not automatically change when the template is edited. The caller is deliberately small and should remain stable; the reusable implementation is the part updated through v1.
- **Deployment is repository-local.** Install the caller only in repositories where native sub-issue automation is needed. Creating the workflow file does not by itself add every Issue to a GitHub Project.
- **Project bookkeeping remains separate.** The Issue is the durable work record; the Project item and its status must be reconciled explicitly.

## Completion criteria

The chosen architecture is implemented, the copy template is available centrally, the command syntax and boundaries are documented, and future agents have an exact installation path. Remaining operational checks are to verify the parent link and Actions execution for each repository where the caller is deployed, and to ensure the corresponding Issue is present in the intended Project with the correct status.
