# Retro workflow

This document describes how to investigate and follow through on Retro Issues. Read it when you take on an existing Retro or are asked to work through Retro items; routine project turns do not require loading it.

Each Retro is a chance to reduce recurring friction and deviations in future work. The aim is to understand enough about the situation to improve the agent's future behavior, decisions or project setup. A complete root-cause explanation is desirable, but never required when evidence or answers are unavailable.

## Status

Keep a `Status` line near the top of every Retro Issue. Use these values:

- `Investigating`
- `Ready`
- `In progress`
- `Resolved`
- `Deferred`

A Retro moves through these states over time. At each turn, take the next useful step for its current status; do not try to complete the whole lifecycle at once.

## Investigating

Reconstruct the situation from the conversation and project context. Check relevant guidance, decisions and earlier Retros. Look beyond the first explanation, using a 5-Whys mindset to find a cause or pattern whose correction could prevent similar cases.

- Separate observed facts from interpretations and hypotheses.
- If it is unclear what to ask, keep examining the context and identify the missing information.
- When a useful question becomes clear, ask it with concise answer options and mark that the Retro is awaiting input.
- When an answer arrives, update the Issue and continue only as far as the new information allows. Ask follow-up questions when they can materially improve understanding.
- Move to `Ready` when the situation is understood well enough to define a useful response. Retain uncertainty explicitly where necessary.

## Ready

Describe the change that should reduce recurrence. Decide whether it can be handled within the Retro or needs a separate implementation Issue. Link related work and move to `In progress` when implementation starts.

## In progress

Track implementation in the Retro or its linked Issue. Record relevant evidence and keep the Retro open until the change is complete and checked. Then describe the outcome and move to `Resolved`.

## Resolved

Record what changed and how it addresses the original friction or deviation. Close the Issue after verifying the outcome. If the same pattern recurs, reopen the Retro or create a linked one and revisit the earlier conclusion.

## Deferred

Use when the Retro is deliberately left without a solution. Record briefly what remains unknown, why work is stopping and what new information could justify revisiting it. A deferred Retro is a valid outcome; do not manufacture a resolution.
