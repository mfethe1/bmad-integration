# Inversion
**Method:** Backward-plan failure to illuminate the path to success  
**Origin:** Carl Jacobi / Charlie Munger / Stoic philosophy  
**Best for:** Goal setting, system design, process improvement, guaranteed outcomes

---

## What It Is

Inversion flips the question. Instead of asking **"how do I succeed?"**, inversion asks **"what would guarantee failure?"** — then you avoid those conditions.

The insight: forward-planning success is hard because success has many paths and you can't know which will work. Backward-planning failure is often easier because failures have identifiable, specific causes that can be systematically eliminated.

**Jacobi's principle:** "Invert, always invert."  
**Munger's application:** "I only want to know where I'm going to die, so I never go there."

---

## When to Run It

- When trying to guarantee a specific outcome (reliability, security, performance)
- When designing a process that must not fail (CI/CD, cron jobs, health checks)
- When you're optimizing a system and keep hitting diminishing returns (invert to find what's dragging it down)
- When goal-setting feels vague ("be more reliable" → invert to concrete failure conditions)
- When your forward plan has too many options and you can't choose

---

## Template

```markdown
# Inversion Analysis: [Goal/Outcome]
**Date:** YYYY-MM-DD
**Goal:** [The thing you're trying to achieve — be specific]
**Author:** [Agent name]

---

## Phase 1: Define the Inverted Goal

Instead of: "[Original goal]"
Ask: "How would I guarantee that [original goal] NEVER happens?"

**Inverted goal:** [Write the opposite of what you want]

Example:
- Original: "Deploy reliably to production with zero downtime"
- Inverted: "Guarantee production is down or corrupted after every deploy"

---

## Phase 2: Generate Failure Conditions

How would you guarantee the inverted goal? Be creative and specific.
These are the conditions you MUST avoid.

"To guarantee [inverted goal], I would..."

**Process failures:**
1. 
2. 
3. 

**Technical failures:**
1. 
2. 
3. 

**Coordination failures:**
1. 
2. 
3. 

**Environmental failures:**
1. 
2. 
3. 

**Time-based failures (what degrades over time?):**
1. 
2. 
3. 

---

## Phase 3: Map Failure Conditions to Prevention

For each failure condition, design the prevention:

| Failure Condition | How to Prevent | How to Detect if Happening | How to Recover |
|------------------|---------------|--------------------------|----------------|
| | | | |
| | | | |
| | | | |

---

## Phase 4: Priority Rank by Likelihood × Impact

Sort the failure conditions by: which is most likely to happen AND has the highest impact?

| Rank | Failure Condition | Likelihood (1-5) | Impact (1-5) | Priority |
|------|------------------|------------------|--------------|----------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |

---

## Phase 5: Build the "Anti-Plan"

Now write a set of rules — the **anti-plan** — that defines what NEVER to do:

"To succeed at [original goal], I must NEVER:"
1. [Anti-rule derived from top failure condition]
2. [Anti-rule]
3. [Anti-rule]
4. [Anti-rule]
5. [Anti-rule]

---

## Phase 6: Convert Anti-Plan to Positive Checklist

Take each "never do X" and convert to "always do Y instead":

| Anti-Rule | Positive Implementation |
|-----------|------------------------|
| NEVER deploy without running tests | ALWAYS run test suite and verify green before merge |
| NEVER rotate secrets without health check | ALWAYS verify new secret works before invalidating old |
| NEVER push directly to main | ALWAYS use PR with review gate |

---

## Output

```
Inversion Analysis Complete

Failure conditions identified: N
High-priority conditions: [list top 3]
Anti-rules defined: N
Prevention checklist: [attached below or linked]

Confidence that following anti-plan prevents failure: [HIGH | MEDIUM | LOW]
If LOW: identify which failure condition cannot be fully prevented and document accepted risk.
```

**Prevention checklist:**
- [ ] [Positive rule 1]
- [ ] [Positive rule 2]
- [ ] [Positive rule 3]
- [ ] [...]
```

---

## Example: "Never lose agent context"

**Goal:** Ensure agents always have current context when starting a session.

**Inverted:** "Guarantee agents always start with stale or missing context."

**Failure conditions generated:**
1. Never write context to persistent storage — keep it all in volatile conversation memory
2. Use inconsistent file names so agents can't find context files
3. Write context to Edrive only, with no fallback when Edrive is unavailable
4. Update context at end of session but never at checkpoints during long sessions
5. Have multiple agents overwrite the same context file simultaneously without coordination
6. Use timestamps inconsistently so agents can't tell which file is current
7. Let context files grow without limit until they're too large to load efficiently

**Top prevention rules derived:**
1. NEVER keep critical state only in conversation memory
2. ALWAYS use the 3-tier path resolution (Edrive → local → workspace)
3. ALWAYS checkpoint context every 2-3 minutes in long sessions
4. ALWAYS use file locks or agent-specific files for concurrent writes
5. ALWAYS write timestamps consistently (ISO 8601 in UTC)
6. NEVER let a context file exceed 50KB without archiving old content

---

## Advanced: Inversion for Agent Coordination

Useful inversion prompts for the 4-agent system:

**"How would I guarantee our agents never coordinate effectively?"**
→ Gives you the anti-patterns to avoid in AGENTS.md

**"How would I guarantee our cron jobs always fail?"**
→ Gives you the checklist for the Cron Job DoD

**"How would I guarantee our QA reviews miss critical bugs?"**
→ Gives you the adversarial review protocol requirements

**"How would I guarantee context is always lost between sessions?"**
→ Gives you the memory hygiene rules

---

*Write inversion output to `/tmp/inversion-<goal>.md`. Anti-rules become checklist items.*
