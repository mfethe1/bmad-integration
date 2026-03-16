# 3-Layer Adversarial Code Review

A multi-perspective code review system that spawns 3 parallel reviewers with different information access levels to maximize defect detection through information asymmetry.

## Overview

Traditional single-pass code reviews suffer from confirmation bias — the reviewer sees the same context as the author and makes the same assumptions. This skill eliminates that by running 3 independent review layers in parallel, each with deliberately different context windows.

## Architecture

### Layer 1: Blind Hunter (No Context)
- Receives ONLY the diff — no project files, no specs, no history
- Finds: obvious bugs, logic errors, style violations, security issues visible in isolation
- Persona: "Cynical, jaded reviewer with zero patience for sloppy work"
- Must find at least 10 issues or HALT (zero findings = suspicious)

### Layer 2: Edge Case Hunter (Diff + Project Access)
- Receives the diff AND read access to project files
- Finds: unhandled paths, missing guards, boundary conditions, race conditions
- Method: exhaustive path enumeration (mechanically walk every branch, not hunt by intuition)
- Output: structured JSON array with `location`, `trigger_condition`, `guard_snippet`, `potential_consequence`

### Layer 3: Acceptance Auditor (Diff + Spec + Context)
- Receives the diff, the spec/story file, and project context docs
- Finds: AC violations, spec deviations, missing behavior, contradictions
- Only runs when a spec file is available (`full` mode)

## Usage

### Inputs
- `diff` (required) — Git diff output or file changes to review
- `spec_file` (optional) — Path to the spec/story/AC file. If provided, runs in `full` mode with all 3 layers. If omitted, runs in `no-spec` mode (layers 1 + 2 only).
- `also_consider` (optional) — Additional areas to focus on during review

### Invocation
```
Run adversarial review on <diff> [against spec <spec_file>]
```

### Output
A triaged findings report with:
- Each finding numbered (F1, F2, F3...)
- Severity: Critical / High / Medium / Low
- Source layer: Blind / Edge-Case / Acceptance
- Validity: Real / Noise / Undecided
- Deduplicated across layers (same issue found by multiple layers = higher confidence)

## Integration with Our System

This skill is designed to be called by:
1. **coding-agent** — after implementation, before marking a story done
2. **gh-issues** — when reviewing PRs
3. **Manual invocation** — `run adversarial review on <branch>`

## Step Files
- `steps/step-01-construct-diff.md` — Build the diff from git or file changes
- `steps/step-02-spawn-reviewers.md` — Launch 3 parallel subagents
- `steps/step-03-triage-findings.md` — Deduplicate, severity-rank, present results
- `steps/step-04-resolve-findings.md` — User chooses which findings to fix

## HALT Conditions
- Any layer returns zero findings → suspicious, re-analyze
- Diff is empty → abort
- All 3 layers fail → escalate to human
