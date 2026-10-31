# AGENTS.md

- To manage native GitHub sub-issues, use `<!-- issue-parent: #N -->` or `<!-- issue-parent: none -->` in the child Issue body; when working in another repository, bootstrap it with `.github/workflows/issue-parent-sync.yml` listening to `issues: [opened, edited]`, granting `permissions: issues: write`, and calling `vernikr/mother/.github/workflows/issue-parent-sync.yml@v1`.

## Retro

- Treat a user message as a Retro candidate when it departs from the expected flow: confusion, friction, objection, unexpected suggestion, unexplained choice, change of direction or unclear intent. Do not limit candidates to explicit complaints; do not turn every routine exchange into a Retro.
- Before recording, review the surrounding conversation, relevant project context and existing Retro Issues. Create or update a `[Retro]` Issue in this repository. No separate Retro Project or physical `retro/` directory.
- Write briefly and naturally: what the agent proposed, what the user replied, and enough context for a future agent to understand the deviation. Avoid rigid `Why / Action / Future` sections.
- Investigate beyond the first explanation. Use a 5-Whys mindset to find a reusable cause or broader pattern; inspect evidence and prior Retros. Separate facts from hypotheses. Do not invent a root cause or force certainty.
- If intent or cause remains unclear, think through the available context first, then ask focused questions with answer options. Keep the Issue open while answers or implementation are pending; an unanswered question is still useful context.
- Keep a semantic `Status` in the Issue body: `Investigating` (questions being formed / awaiting answers / answers received), `Ready` (solution is clear), `In progress`, `Resolved`, or `Deferred` (deliberately left unresolved).
- Retro may track implementation. If work needs a separate Issue, link it from the Retro and record the outcome there. Technical gotchas belong in relevant project documentation; `learning` and `improvement` are possible outcomes, not Retro categories.
