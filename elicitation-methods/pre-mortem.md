# Pre-Mortem
**Method:** Prospective Hindsight — "Imagine this failed. Why?"  
**Origin:** Gary Klein / cognitive psychology  
**Best for:** Planning, launch readiness, complex multi-step workflows

---

## What It Is

A pre-mortem travels to a **future where the plan already failed** — then works backward to identify why.

Unlike risk assessments (which ask "what could go wrong?"), a pre-mortem assumes failure is certain and asks "what happened?" This small cognitive shift dramatically increases the specificity and realism of failure identification.

Prospective hindsight — imagining a future event as already having happened — increases the ability to correctly identify reasons for future outcomes by 30%.

---

## When to Run It

- Before implementing a complex feature or system change
- Before launching any cron job that modifies production data
- Before any irreversible action (secret rotation, DB migration, config change)
- Before shipping work that multiple other agents or systems depend on
- Any time you feel "this is probably fine"

---

## Template

```markdown
# Pre-Mortem: [Task/Plan Name]
**Date:** YYYY-MM-DD
**Task:** [Brief description of what you're about to do]
**Author:** [Agent name]

---

## Setup: The Scenario

It is [DATE + 2 weeks from now]. The task is complete and in production.
It has catastrophically failed.

The failure is real. Users are affected (or: data is corrupt, or: the system is down, or: revenue stopped).
The team is doing a postmortem.

**This is not hypothetical. The failure happened. Investigate it.**

---

## Phase 1: Generate Failure Scenarios (5 min — don't filter, just generate)

Brainstorm everything that could have caused the failure. Go fast. No idea is too obvious or too paranoid.

*Requirements failures:*
1. 
2. 
3. 

*Implementation failures:*
1. 
2. 
3. 

*Integration failures (what did this break around it?):*
1. 
2. 
3. 

*Operational failures (what broke after it shipped?):*
1. 
2. 
3. 

*Agent/coordination failures (what went wrong in how the work was done?):*
1. 
2. 
3. 

---

## Phase 2: Score by Probability × Impact

Rate each failure scenario:
- **Probability:** 1 (unlikely) → 5 (almost certain)
- **Impact:** 1 (minor) → 5 (catastrophic)
- **Priority:** P × I

| Scenario | Probability | Impact | Priority | Preventable? |
|----------|-------------|--------|----------|-------------|
| | | | | |
| | | | | |
| | | | | |

---

## Phase 3: Targeted Prevention

For the top 3-5 highest-priority scenarios:

### Failure: [scenario name]
**Why it happens:** [root cause chain]
**Warning signs:** [early indicators we could detect]
**Prevention:** [specific action to prevent this before implementation]
**Detection:** [how to catch it if it happens despite prevention]
**Recovery:** [what to do if it happens in production]

---

## Phase 4: Plan Adjustments

Based on the pre-mortem, what changes to the implementation plan?

**Changes to make before starting:**
- [ ] 
- [ ] 

**Monitoring/alerting to add:**
- [ ] 

**Rollback triggers to define:**
- [ ] 

**Acceptance criteria to add (or tighten):**
- [ ] 

---

## Output

```
Pre-Mortem Complete: [PLAN UNCHANGED | MINOR ADJUSTMENTS | MAJOR REVISION NEEDED]

Top risks identified: N
High-priority scenarios: [list top 3]
Plan changes required: [list key changes]
Confidence level post-mortem: [HIGH | MEDIUM | LOW]

If LOW confidence: escalate before proceeding.
```
```

---

## Example: Cron Job Deployment

**Setup:** Deploying a new cron job that rotates API credentials every 24 hours.

**Failure scenarios generated:**
1. Job runs, rotates key, but fails to update the environment variable in Railway — all API calls break
2. Job runs during peak traffic hours — old key invalidated before new key is live (gap window)
3. Job fails silently — credentials expire, system breaks 30 days later when nobody remembers the job
4. Rotation works but old key was still in use by a second service that wasn't updated
5. Job runs twice due to a scheduling bug — second rotation invalidates the first new key

**Highest priority prevention:**
- Deployment order: update env var FIRST, then invalidate old key (never the reverse)
- Add a post-rotation health check that verifies the new key works before invalidating the old
- Add a 3-failure alert policy — if job fails 3 times, page immediately
- Audit all services using the credential before running the job for the first time

---

*Run this before every high-stakes implementation. Write the output to `/tmp/premortem-<task>.md` and reference it in the Implementation Readiness Gate.*
