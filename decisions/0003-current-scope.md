# 0003 — Current scope boundaries

**Status:** working; pending explicit owner approval.

## Working decision

- `state.md` is deferred for this repository and phase.
- `HANDOFF.md` is the current operational handoff for `mother`.
- `FEATURES.md` is the proposed root feature-tree artifact, but its universal
  contract is still under research.
- Feature-tree `FR-ID` values are optional until their value is validated.
- Blockers are Issue-owned by default rather than a parallel blocker registry.
- New research is intended for `docs/research/`; the current
  `research/agent-first/` directory remains the historical home of this phase.
- Top-level `decisions/` is the intended canonical location for durable decision
  records.

## Consequence

Agents should not create `state.md`, a blocker registry, mandatory FR-IDs or a
second decision location as part of routine work without a new owner decision.
