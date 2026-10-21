# Mother conformance guide

This file is the short entry point for an agent asked to bring a project into
alignment with the methodology described by `vernikr/mother`.

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
- platform, UI, service, research or unattended-operation extensions.

Record missing pieces, contradictions and obsolete documents before editing.

## 3. Work from an Issue

Create or use one GitHub Issue for the conformance work. Split substantial
independent profiles into sub-issues. Use a repository plan only when the work
spans sessions, repositories, external systems or difficult verification.

Do not create a parallel master backlog in `TODO.md`. Keep the Issue and Project
as the work-management source of truth and keep durable project knowledge in
the repository.

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

## 6. Stop and ask the owner when

- the target project needs a new methodology rule rather than an application of
  an existing one;
- two documents or systems claim the same source of truth;
- migration would remove information or change a public contract;
- the applicable profile is unclear;
- required external access or a destructive action is unavailable.

Finish with a short human-facing report: changed files, selected profiles,
checks and evidence, remaining gaps, and the next recommended step.
