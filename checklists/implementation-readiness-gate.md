# Implementation Readiness Gate
**Version:** 1.0  
**Owner:** Lenny (QA/Quality)  
**When to run:** BEFORE any implementation begins — this is a pre-flight check, not a retrospective

---

## Purpose

This gate prevents wasted implementation effort caused by:
- Ambiguous or incomplete requirements
- Missing dependencies (API keys, services, access)
- Insufficient context (architecture unknown, codebase unfamiliar)
- Scope too large for a single session/PR
- Risk not acknowledged before irreversible changes

**An agent that skips this gate and hits a wall halfway through implementation has failed.**  
**An agent that runs this gate and surfaces a concern has succeeded.**

---

## Gate Dimensions

Run all 6 dimensions. Score each. Compute final verdict.

---

### Dimension 1: Requirements Clarity

*Can the implementation be fully specified from what's available?*

| Check | Status | Notes |
|-------|--------|-------|
| The task has a clear, unambiguous end state | ⬜ PASS / ⬜ FAIL | |
| Acceptance criteria are explicit and testable | ⬜ PASS / ⬜ FAIL | |
| Success is measurable (not "make it better") | ⬜ PASS / ⬜ FAIL | |
| Edge cases and error scenarios are addressed in spec | ⬜ PASS / ⬜ FAIL | |
| Non-functional requirements (perf, security, scale) are specified | ⬜ PASS / ⬜ FAIL | |
| No contradictions between the task description and context docs | ⬜ PASS / ⬜ FAIL | |

**Clarity Score:** N/6

**Red flags that force FAIL or CONCERNS:**
- Spec says "should" or "might" for core behavior (ambiguous)
- No defined output format for the deliverable
- Requirements contradict team standards (AGENTS.md, SOUL.md)
- Spec references files or systems that don't exist

---

### Dimension 2: Dependency Availability

*Is everything needed actually available right now?*

| Check | Status | Notes |
|-------|--------|-------|
| All required APIs/services are accessible (test a call) | ⬜ PASS / ⬜ FAIL | |
| All required credentials/secrets are available in the environment | ⬜ PASS / ⬜ FAIL | |
| Required libraries/packages are installed (check versions) | ⬜ PASS / ⬜ FAIL | |
| External services required are not in a maintenance window | ⬜ PASS / ⬜ FAIL | |
| File system paths required are accessible (Edrive, local mirror, etc.) | ⬜ PASS / ⬜ FAIL | |
| Required agent capabilities/skills are available | ⬜ PASS / ⬜ FAIL | |

**Dependency Score:** N/6

**Red flags:**
- Any required secret is missing or expired
- External service returning 5xx during dependency check
- Required package not installed and internet access limited
- Edrive unavailable AND task requires Edrive-only data with no fallback

---

### Dimension 3: Context Sufficiency

*Does the implementing agent have enough understanding to act correctly?*

| Check | Status | Notes |
|-------|--------|-------|
| Relevant codebase sections have been read (not assumed) | ⬜ PASS / ⬜ FAIL | |
| Architecture decisions that affect this task are understood | ⬜ PASS / ⬜ FAIL | |
| Prior related work is identified (no re-inventing existing patterns) | ⬜ PASS / ⬜ FAIL | |
| memU queried for prior context on this domain | ⬜ PASS / ⬜ FAIL | |
| Team conventions for this type of task are known | ⬜ PASS / ⬜ FAIL | |
| Last time this area was touched is known (no stale assumptions) | ⬜ PASS / ⬜ FAIL | |

**Context Score:** N/6

**Red flags:**
- Haven't read the files that will be modified
- Assuming a pattern exists without verifying it does
- No memory search performed before starting novel work
- Architecture doc says "pending" for relevant section

---

### Dimension 4: Risk Assessment

*What can go wrong, and is the risk acceptable?*

| Risk Category | Level | Mitigation |
|--------------|-------|-----------|
| Data loss risk (could this corrupt or delete data?) | ⬜ None / ⬜ Low / ⬜ Med / ⬜ HIGH | |
| Security risk (new attack surface, credential exposure) | ⬜ None / ⬜ Low / ⬜ Med / ⬜ HIGH | |
| Breaking change risk (other systems/agents depend on this) | ⬜ None / ⬜ Low / ⬜ Med / ⬜ HIGH | |
| Reversibility (can this be undone if it goes wrong?) | ⬜ Fully / ⬜ Partial / ⬜ Hard / ⬜ IRREVERSIBLE | |
| Production impact (does this affect live systems during implementation?) | ⬜ None / ⬜ Low / ⬜ Med / ⬜ HIGH | |
| Revenue impact (could this cause downtime, billing errors, or lost conversions?) | ⬜ None / ⬜ Low / ⬜ Med / ⬜ HIGH | |

**Risk Flag Rules:**
- Any `HIGH` category → automatic CONCERNS (document mitigation before proceeding)
- `IRREVERSIBLE` → automatic CONCERNS + require explicit documented rollback plan
- 2+ `HIGH` categories → automatic FAIL (break into smaller, safer pieces or escalate)
- Revenue impact `HIGH` → escalate to Michael before proceeding

---

### Dimension 5: Scope Validation

*Is this one clear deliverable, or does it need to be split?*

| Check | Status | Notes |
|-------|--------|-------|
| The task can be completed in one session (<2 hours of agent work) | ⬜ PASS / ⬜ FAIL | |
| The task produces exactly one primary deliverable (not N separate things) | ⬜ PASS / ⬜ FAIL | |
| The task does not require parallel agent work to complete | ⬜ PASS / ⬜ FAIL | |
| The task boundary is clear (explicit start and stop) | ⬜ PASS / ⬜ FAIL | |
| The task does not have hidden prerequisites that aren't tracked | ⬜ PASS / ⬜ FAIL | |

**Scope Score:** N/5

**Splitting triggers:**
- Task requires changes to 5+ unrelated files across 2+ domains
- Task includes "and also" clauses that are independent features
- Task estimated >2 hours without natural checkpoints
- Task has hard prerequisite work that isn't done yet

**If splitting is needed:** document the proposed split (Task A → Task B → Task C) before proceeding. Do not implement partially and stop mid-task without a checkpoint artifact.

---

### Dimension 6: Final Confidence Assessment

After evaluating all 5 dimensions above, make an honest confidence assessment:

**Questions to answer:**

1. If I started implementing right now, what is the most likely thing to block me?
2. What assumption am I making that is most likely to be wrong?
3. What is the worst realistic outcome if implementation goes sideways?
4. Is there any part of this task I don't understand well enough to implement correctly?

Write honest answers below:

```
Likely blocker: [answer]
Riskiest assumption: [answer]  
Worst realistic outcome: [answer]
Uncertain areas: [answer]
```

---

## Output Format

### PASS

```
IMPLEMENTATION READINESS GATE: PASS ✅

Confidence Score: [HIGH 90-100% | MEDIUM 70-89% | ACCEPTABLE 60-69%]

Requirements Clarity:  N/6
Dependency Availability: N/6
Context Sufficiency: N/6
Risk Level: [LOW | MEDIUM]
Scope: [SINGLE DELIVERABLE | CONFIRMED FIT FOR ONE SESSION]

Notes:
- [Any minor concerns noted but not blocking]
- [Assumptions documented]

Approved to proceed. Recommended first action: [specific next step]
```

---

### CONCERNS

```
IMPLEMENTATION READINESS GATE: CONCERNS ⚠️

Confidence Score: [40-59% — proceed only after addressing concerns]

Requirements Clarity:  N/6
Dependency Availability: N/6
Context Sufficiency: N/6
Risk Level: [MEDIUM-HIGH — specific risks listed]
Scope: [NEEDS CLARIFICATION | RECOMMEND SPLIT]

Open Concerns (must address before proceeding):
1. [Specific concern with resolution path]
2. [Specific concern with resolution path]
3. [...]

Recommended action: [Resolve concern X by doing Y, then re-run gate]
Proceed only if: [conditions that make it safe to continue without full resolution]
```

---

### FAIL

```
IMPLEMENTATION READINESS GATE: FAIL 🔴

Confidence Score: [<40% — do not implement until issues resolved]

Requirements Clarity:  N/6 ← BELOW THRESHOLD
Dependency Availability: N/6 ← BELOW THRESHOLD
Context Sufficiency: N/6 ← BELOW THRESHOLD

Missing items that MUST be resolved before implementation:
1. [Specific missing item — who needs to provide it]
2. [Specific missing item — how to obtain it]
3. [...]

Risk blockers:
- [Irreversible action without rollback plan]
- [HIGH revenue risk without mitigation]

Scope issues:
- [Task is too large — proposed split: A → B → C]

Do NOT implement until ALL items above are resolved.
Escalation: [who to contact / what to do]
```

---

## Threshold Reference

| Dimension | PASS | CONCERNS | FAIL |
|-----------|------|----------|------|
| Requirements Clarity | ≥5/6 | 3-4/6 | <3/6 |
| Dependency Availability | ≥5/6 | 4/6 | <4/6 |
| Context Sufficiency | ≥4/6 | 3/6 | <3/6 |
| Risk Level | LOW | MEDIUM | HIGH or IRREVERSIBLE |
| Scope | Single deliverable | Clarification needed | Must split |

Overall verdict:
- All PASS → **PASS**
- 1-2 CONCERNS, no FAIL → **CONCERNS**
- Any FAIL → **FAIL**

---

## When to Re-Run the Gate

- After resolving any CONCERNS blocker
- After scope split (each sub-task gets its own gate run)
- After >24 hours have passed since last gate run (context may have changed)
- After any major new information arrives mid-implementation (pivot trigger)

---

*This gate is mandatory for all non-trivial tasks. "Non-trivial" = >30 minutes of work OR any external write action.*
