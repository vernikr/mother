# 0001 — Document authority and precedence

**Status:** working; pending explicit owner approval.

## Question

How should an agent resolve conflicting statements across the repository's
brief, owner direction, research, proposal, handoff and operational guide?

## Working decision

Use this precedence order:

1. explicit owner decision or a linked `approved` decision record;
2. `MOTHER.md` for the current operational conformance contract;
3. current `working` decisions in `decisions/`;
4. the current proposal for design that is not yet policy;
5. owner-direction notes for unresolved intent and constraints;
6. research reports for evidence and alternatives;
7. `HANDOFF.md` for execution state and next actions, never for changing policy;
8. the original context for historical goals and constraints.

When two sources still conflict, preserve both interpretations, stop before a
policy-changing action, and ask the owner.

## Consequence

Historical research must be labeled as historical or superseded. Handoff text
should link to canonical sources instead of restating their full policy.
