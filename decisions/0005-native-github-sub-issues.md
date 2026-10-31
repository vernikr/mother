# 0005 — Native GitHub sub-issues and central workflow

> **Status:** approved  
> **Date:** 2026-10-31  
> **Source:** owner decision and [Issue #24](https://github.com/vernikr/mother/issues/24)

## Decision

Use GitHub's native parent/sub-issue relationship, managed through a central reusable GitHub Actions workflow.

The canonical implementation is .github/workflows/issue-parent-sync.yml in vernikr/mother. The canonical caller template is .github/workflow-templates/issue-parent-sync.yml in the same repository.

A target repository that needs this capability must contain the caller at .github/workflows/issue-parent-sync.yml. When absent, an agent fetches the canonical template from:

    https://raw.githubusercontent.com/vernikr/mother/main/.github/workflow-templates/issue-parent-sync.yml

and copies it verbatim. Agents must not author or reconstruct this YAML manually.

The caller invokes the central workflow through vernikr/mother/.github/workflows/issue-parent-sync.yml@v1. Publish implementation updates to the v1 tag.

## Command contract

- <!-- issue-parent: #NUMBER --> — set or move the child Issue under the specified parent.
- <!-- issue-parent: none --> — detach the child from its current parent.

The command is a standalone line in the child Issue body. The workflow processes it on Issue creation or edit and removes the marker after success. Parent and child must be Issues in the same repository. Self-parenting and pull-request parents are rejected. The current implementation only accepts events sent by vernikr-personal.

Agents must verify the Actions run and the resulting native parent relationship.

## Rationale

- Native GitHub hierarchy keeps relationships queryable and visible in GitHub Projects.
- Central implementation avoids copying executable logic into every repository.
- A canonical caller template removes YAML authoring from the agent workflow and reduces installation errors.
- A single global AGENTS.md remains the source of routine agent instructions; repository-local instruction copies are intentionally avoided.

## Consequences and constraints

- Each target repository still needs a local caller because GitHub events originate in that repository.
- The v1 reference is the shared implementation release point; implementation changes must be published there.
- A copied caller is a stable snapshot and does not auto-update when the template changes.
- GitHub Actions runner billing/quota is charged to the caller repository; reuse does not transfer usage to mother.
- The current implementation authorizes only vernikr-personal.
- Project membership and status are separate from the native parent relationship and must be reconciled explicitly.

## Implementation

- Reusable workflow: [ .github/workflows/issue-parent-sync.yml ](../.github/workflows/issue-parent-sync.yml)
- Caller template: [ .github/workflow-templates/issue-parent-sync.yml ](../.github/workflow-templates/issue-parent-sync.yml)
- Retrospective: [native GitHub sub-issue workflow](../research/agent-first/03-github-sub-issue-workflow.md)
- Work item: [Issue #24](https://github.com/vernikr/mother/issues/24)
