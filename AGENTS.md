# AGENTS.md

- To manage native GitHub sub-issues, use `<!-- issue-parent: #N -->` or `<!-- issue-parent: none -->` in the child Issue body; when working in another repository, bootstrap it with `.github/workflows/issue-parent-sync.yml` listening to `issues: [opened, edited]`, granting `permissions: issues: write`, and calling `vernikr/mother/.github/workflows/issue-parent-sync.yml@v1`.

## Retro

Retro exists to reduce recurring friction and deviations from the expected flow. Capture moments that reveal a gap in the agent's understanding, decisions or working method, then use what is learned to make similar situations less likely in the future.

Create or update a `[Retro]` Issue when a user message signals confusion, friction, objection, an unexpected suggestion, an unexplained choice, a change of direction or unclear intent. Review the conversation, project context and existing Retros first. Write a brief, natural account of what the agent proposed and how the user responded. Keep observed facts distinct from possible explanations.

Work according to the Retro's current status. At each turn, take the next useful step for that status; the whole lifecycle does not need to be completed at once. Investigate before asking, and make questions focused with answer options.

### Investigating

Reconstruct what happened and why. Check relevant project guidance, prior decisions and similar Retros. Explore beyond the first explanation, using a 5-Whys mindset to look for a cause or pattern that could prevent a wider class of recurrence. Treat conclusions as hypotheses until supported.

- If it is not yet clear what to ask, continue examining the context and identify the missing information.
- When useful questions become clear, ask them with answer options and mark that input is awaited.
- When answers arrive, update the Retro and continue the investigation. Ask follow-up questions only when they can materially improve understanding.
- Move to `Ready` when the cause is understood well enough and a useful response is clear. It is acceptable to retain uncertainty.

### Ready

Describe the change that should reduce recurrence. Decide whether it belongs in the Retro itself or needs a separate implementation Issue, and link related work. Move to `In progress` when implementation starts.

### In progress

Track the implementation in the Retro or its linked Issue. Keep the Retro open until the change is complete and checked. Record the outcome, then move to `Resolved`.

### Resolved

Record what changed and how it addresses the original friction or deviation. Close the Issue when the outcome has been verified. Reopen or create a linked Retro if the same pattern recurs.

### Deferred

Use when the Retro is deliberately left without a solution. Briefly record what remains unknown and why work is stopping. Preserve the context so the investigation can resume if new information appears.
