# Adversarial Review Protocol
**Version:** 1.0  
**Owner:** Lenny (QA/Quality)  
**Status:** ACTIVE — all agents must follow this protocol for any review task

---

## Core Mandate

> **REVIEWERS MUST FIND ISSUES.**
>
> Zero findings is NOT a valid output. Zero findings means insufficient analysis.
>
> If you genuinely believe no issues exist, you must **prove it with evidence**: test output, static analysis results, explicit reasoning for each category checked. "Looks fine" is a rejection, not a finding.

This is not pessimism — it is discipline. Confirmation bias is the most common QA failure mode. The adversarial stance counteracts it by treating "find problems" as the primary objective.

---

## The Three-Findings Rule

Every adversarial review **MUST** produce at minimum:
- **3 findings** (any severity: Critical → Nit), OR
- **Explicit evidence-backed justification** for why each standard finding category is clean

If a reviewer submits fewer than 3 findings without evidence, the review is **rejected and re-run**.

---

## Reviewer Personas

Three parallel personas run simultaneously on every code/system review. Each has a distinct information boundary and attack vector.

---

### Persona 1: Blind Hunter 🎯

**Role:** Pure code logic reviewer. No context. No spec. No mercy.

**Information boundary:** DIFF ONLY. No project context, no requirements, no architecture docs.

**Mission:** Find bugs, logic errors, and code quality issues purely from what the code says — not what it was meant to say.

**Attack surface:**
- Off-by-one errors, null/undefined dereferences
- Unreachable code, dead branches
- Type coercions and implicit conversions
- Resource leaks (unclosed handles, forgotten cleanup)
- Incorrect boolean logic (De Morgan violations, operator precedence)
- Race conditions visible from the diff alone
- Missing error propagation
- Hardcoded values that should be configurable
- Copy-paste errors, inconsistent naming

**Stance:** "This code will fail. Find where."

**Minimum findings:** 3. If the diff is genuinely clean, prove it by listing the 5 highest-risk patterns you checked and confirming each was not present with line-level evidence.

---

### Persona 2: Edge Case Hunter 🔬

**Role:** Integration and boundary tester. Sees the whole codebase.

**Information boundary:** DIFF + full repo read access.

**Mission:** Find what breaks at the boundaries — inputs the author didn't consider, integration points that mismatch, race conditions that emerge at scale.

**Attack surface:**
- Empty input, null input, max-length input, unicode/emoji
- Concurrent access to shared state (mutex, lock, transaction gaps)
- Retry loops that can double-apply side effects
- API contract mismatches between caller and callee
- Database constraint violations not caught at app layer
- Timeout and retry edge cases
- Memory/CPU spikes on large payloads
- Cascading failure scenarios (what breaks downstream if this fails?)
- Backward compatibility breaks in data schemas
- Environment-specific assumptions (timezone, locale, file path separator)

**Stance:** "The happy path passes. Find where it breaks."

**Minimum findings:** 3. If no edge cases found, list the top 5 boundaries checked with test evidence or reasoning.

---

### Persona 3: Acceptance Auditor 📋

**Role:** Requirements validator. Does the implementation actually fulfill the spec?

**Information boundary:** DIFF + spec/story/ticket + context docs (AGENTS.md, active-context.md, task description).

**Mission:** Find gaps between what was asked for and what was built.

**Attack surface:**
- Acceptance criteria not implemented or partially implemented
- Behavior that contradicts spec constraints
- Missing UX/API contract behavior described in spec
- Business rules bypassed or approximated
- Non-functional requirements ignored (perf thresholds, SLA, accessibility)
- Security requirements from the spec not enforced
- Error messages that don't match spec-defined user-facing strings
- Scope creep — code that implements things not in the spec (risk surface)
- DoD items not satisfied

**Stance:** "The spec said X. Does the code do X? Prove it."

**Minimum findings:** 3. If all ACs are met, enumerate each AC and reference the specific code that satisfies it.

---

## Severity Classification

| Severity | Definition | Action Required |
|----------|-----------|-----------------|
| **Critical** | Data loss, security breach, auth bypass, crash in production path, revenue failure | Block ship. Fix before merge. |
| **Major** | Incorrect behavior in non-trivial cases, missing error handling for common failures, perf regression >20% | Must fix in current PR or create tracked issue with ETA |
| **Minor** | Edge case mishandling, suboptimal logic, missing test coverage for secondary paths | Fix or defer with explicit decision |
| **Nit** | Style, naming, docs, minor readability | Optional — author decides |

---

## Output Format Template

Every adversarial review MUST use this format:

```markdown
# Adversarial Review: [PR/Task/Commit ID]
**Reviewer:** [Blind Hunter | Edge Case Hunter | Acceptance Auditor]
**Date:** YYYY-MM-DD
**Stance:** ADVERSARIAL — finding issues is the primary objective

---

## Summary
- Total findings: N
- Critical: X | Major: Y | Minor: Z | Nit: W
- Overall recommendation: [BLOCK | APPROVE WITH FIXES | APPROVE]

---

## Findings

### Finding 1 — [One-line title]
**Severity:** Critical | Major | Minor | Nit
**Location:** `path/to/file.py:line_number`
**Category:** [logic | edge-case | security | perf | requirements | docs]
**Evidence:**
```
[paste relevant code or output]
```
**Impact:** What breaks, for whom, under what conditions.
**Fix recommendation:** Specific, actionable suggestion.

### Finding 2 — [One-line title]
[... same format ...]

---

## Categories Checked (for "clean" justification)
If fewer than 3 findings, list every category checked with evidence:
- [ ] Null/undefined handling — checked lines 45, 67, 103 — CLEAN (explicit null guards present)
- [ ] Error propagation — checked all async paths — CLEAN (all await wrapped in try/catch)
- [...]

---

## Triage Classification
| Finding | Classification | Rationale |
|---------|---------------|-----------|
| Finding 1 | patch | Fixable in this PR |
| Finding 2 | defer | Pre-existing issue, not introduced here |
| Finding 3 | intent_gap | Spec doesn't cover this case — spec needs update |
| Finding N | reject | False positive — behavior is correct because... |
```

---

## Triage Classification Definitions

| Class | Meaning |
|-------|---------|
| `patch` | Issue introduced by this change; fix in current PR |
| `defer` | Pre-existing issue not introduced here; create tracked ticket |
| `intent_gap` | Missing spec/requirements; spec update needed |
| `reject` | False positive; reviewer explains with evidence why behavior is correct |

---

## Integration: OpenClaw Subagent Dispatch

### Standard Dispatch Pattern

For any PR or implementation review, spawn all three personas in parallel:

```javascript
// sessions_spawn calls — run simultaneously
const reviewers = await Promise.all([
  sessions_spawn({
    task: BLIND_HUNTER_PROMPT,
    label: "blind-hunter",
    mode: "run",
    runTimeoutSeconds: 300
  }),
  sessions_spawn({
    task: EDGE_CASE_HUNTER_PROMPT,
    label: "edge-case-hunter",
    mode: "run",
    runTimeoutSeconds: 300
  }),
  sessions_spawn({
    task: ACCEPTANCE_AUDITOR_PROMPT,
    label: "acceptance-auditor",
    mode: "run",
    runTimeoutSeconds: 300
  })
]);
```

### Failure Handling

If any reviewer subagent fails or times out:
1. Log failure to `/tmp/review-[id]/failed-layers.txt`
2. Proceed with findings from the remaining reviewers
3. Note missing layer prominently in final report
4. Fallback: write prompt to file, halt, ask human to run manually

### Deduplication

After collecting all three reviews:
1. Identify duplicate findings (same file/line, same root cause)
2. Merge duplicates, keeping the most severe classification
3. Present unified finding list sorted by severity (Critical first)

---

## Example Prompts

### Blind Hunter Prompt
```
ADVERSARIAL STANCE ACTIVE.

You are the Blind Hunter. Your job is to find bugs in this code diff.

RULES:
- You receive ONLY the diff below. No project context. No spec. No exceptions.
- You MUST find at least 3 issues or prove each category is clean with evidence.
- Zero findings = review rejected. Look harder.
- Use the standard adversarial review output format.

DIFF:
---
[paste full git diff here]
---

Begin your adversarial review now. Focus: logic errors, null handling, resource leaks, type errors, missing error propagation, race conditions visible in the diff.
```

### Edge Case Hunter Prompt
```
ADVERSARIAL STANCE ACTIVE.

You are the Edge Case Hunter. Your job is to find what breaks at the boundaries.

RULES:
- You have the diff AND read access to the full codebase.
- You MUST find at least 3 edge cases or boundary failures.
- Zero findings = review rejected. Look at integration points.
- Use the standard adversarial review output format.

DIFF:
---
[paste full git diff here]
---

CODEBASE ROOT: [path/to/repo]

Key areas to probe: empty/null/max inputs, concurrent access, retry idempotency, 
API contract mismatches, cascade failures, environment assumptions.

Begin your adversarial review now.
```

### Acceptance Auditor Prompt
```
ADVERSARIAL STANCE ACTIVE.

You are the Acceptance Auditor. Your job is to find gaps between spec and implementation.

RULES:
- You have the diff, the spec/story below, and context docs.
- You MUST find at least 3 gaps or prove each AC is satisfied with code evidence.
- Zero findings = review rejected unless you enumerate each AC with evidence.
- Use the standard adversarial review output format.

DIFF:
---
[paste full git diff here]
---

SPEC/STORY:
---
[paste task description, story, or ticket here]
---

CONTEXT:
- active-context.md: [relevant excerpts]
- AGENTS.md quality gates: [relevant sections]

Begin your adversarial review. Focus: AC violations, missing behavior, scope gaps, 
spec contradictions, non-functional requirements not enforced.
```

---

## When to Run Adversarial Review

| Trigger | Reviewers Required |
|---------|-------------------|
| PR merge request | All 3 |
| Production hotfix | All 3 (expedited — 15 min max) |
| Infrastructure change | Blind Hunter + Edge Case Hunter |
| New cron job | Edge Case Hunter + Acceptance Auditor |
| Config/secret change | Blind Hunter (security focus) |
| Documentation change | Acceptance Auditor only |
| Agent prompt/skill change | All 3 |

---

## Anti-Patterns (Hard Violations)

| Violation | Consequence |
|-----------|-------------|
| "Code looks good" with no findings | Review rejected, re-run required |
| Findings without file/line evidence | Finding classified as `reject` |
| Fewer than 3 findings with no clean-category proof | Review rejected |
| Triage classification missing | Report incomplete, not accepted |
| Acceptance Auditor reviewing without spec | Run as Edge Case Hunter instead |

---

*This protocol is mandatory for all Lenny QA reviews and recommended for all agent code review tasks.*
