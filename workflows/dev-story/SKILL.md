# Dev Story Workflow — Red-Green-Refactor Enforcement

A strict implementation workflow that enforces test-driven development discipline for every task in a story.

## Overview

This workflow ensures agents follow proper TDD when implementing stories:
1. Read the ENTIRE story/spec before touching any code
2. Execute tasks IN ORDER as written — no skipping, no reordering
3. Write FAILING tests first (Red)
4. Implement MINIMAL code to make tests pass (Green)
5. Refactor while keeping tests green (Refactor)
6. Mark task complete ONLY when ALL tests pass
7. Run adversarial review before marking story done

## Critical Rules (Non-Negotiable)

- **NEVER lie about tests being written or passing** — tests must actually exist and pass
- **NEVER mark a task complete without verified passing tests**
- **NEVER skip the Red phase** — write failing tests first, confirm they fail
- **NEVER proceed to next task with failing tests**
- **Execute continuously** — do not stop for "milestones" or "session boundaries"
- **Only HALT for explicit HALT conditions**, not for progress updates

## Step Files

- `steps/step-01-load-story.md` — Read and parse the full story file
- `steps/step-02-red-green-refactor.md` — TDD implementation loop per task
- `steps/step-03-validate-complete.md` — Definition-of-done checklist
- `steps/step-04-adversarial-gate.md` — Mandatory adversarial review before merge

## Integration

This workflow is called by agents when implementing any story from BACKLOG.md or a spec file. It replaces ad-hoc "just build it" implementation with structured TDD discipline.

## HALT Conditions

- Story file inaccessible or missing
- Task requirements are ambiguous (ask for clarification)
- 3 consecutive implementation failures on same task
- Required configuration/credentials missing
- Adversarial review finds unresolved Critical findings
