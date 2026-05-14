# First Principles Reasoning
**Method:** Decompose to fundamentals, reconstruct without assumptions  
**Origin:** Aristotle / Feynman / Descartes / applied engineering  
**Best for:** Novel problems, breaking out of "we've always done it this way," debugging mysterious failures

---

## What It Is

First principles reasoning strips away inherited assumptions, conventional wisdom, and analogy-based thinking to find the ground truth of a problem — then builds up from there.

Most approaches to problems are **analogy-based**: "this is like X, so we'll do what worked for X." This is fast and often correct. But when X isn't actually like Y, analogy-based reasoning produces confidently wrong answers.

First principles breaks the analogy chain. It asks: **what do we know for certain? What can we prove? What are we just assuming?**

---

## When to Run It

- You're stuck in a debugging loop and the "obvious" fix keeps not working
- You've inherited a system design and you're not sure the design is right
- Multiple agents have tried an approach and all failed in the same way
- The "conventional" solution keeps hitting friction you can't explain
- You're building something novel with no clear prior art

---

## Template

```markdown
# First Principles Analysis: [Problem/Topic]
**Date:** YYYY-MM-DD
**Problem:** [One sentence description of what you're trying to understand or solve]
**Author:** [Agent name]

---

## Phase 1: Identify What You've Been Assuming

List every assumption embedded in your current approach. Be brutally honest.
Assumptions are things you're treating as true without proof.

"We assume..."
1. 
2. 
3. 
4. 
5. 

For each assumption, mark: **[PROVEN | BELIEVED | GUESSED | UNKNOWN]**

---

## Phase 2: Strip Down to Atomic Facts

From everything above, what do you know with certainty?
These are facts that can be verified: tested, measured, read from docs, confirmed from logs.

"We know for certain..."
1. [Fact] — Evidence: [how we verified this]
2. [Fact] — Evidence: [how we verified this]
3. [Fact] — Evidence: [how we verified this]
4. [Fact] — Evidence: [how we verified this]
5. [Fact] — Evidence: [how we verified this]

---

## Phase 3: Identify Constraint Space

Given only the atomic facts above, what constraints exist on valid solutions?

Constraints are things that MUST be true for any solution to work:
- Technical constraints: [what the system can and can't do]
- Resource constraints: [time, compute, tokens, API rate limits]
- Integration constraints: [what other systems require of this one]
- Quality constraints: [what "good enough" means for this problem]

| Constraint | Source | Negotiable? |
|-----------|--------|-------------|
| | | |
| | | |

---

## Phase 4: Reconstruct the Solution from Scratch

Ignoring all prior approaches and analogies, design the simplest solution that:
- Satisfies all constraints from Phase 3
- Uses only facts from Phase 2
- Makes no assumptions from Phase 1

**Ground-up design:**

Step 1: [what the system must do first]
Step 2: [what follows logically]
Step 3: [...]

**Why this is different from the previous approach:**
[Explain specifically what assumptions the old approach made that this one doesn't]

---

## Phase 5: Challenge the Reconstruction

Apply 3 adversarial questions to the new design:

1. **"What if X breaks?"** — where X is the single most critical component
   Answer: 

2. **"What assumption am I still making?"** — there's always one more
   Answer: 

3. **"What would a skeptic say?"** — the most compelling objection
   Answer: 

---

## Output

```
First Principles Analysis Complete

Key assumptions eliminated: [list the BELIEVED/GUESSED ones you dropped]
Ground truth facts relied on: [list the PROVEN facts you built from]
Solution change from prior approach: [describe the key difference]
Confidence in reconstruction: [HIGH | MEDIUM | LOW]

If LOW: identify which fact is uncertain and how to verify it before proceeding.
```
```

---

## Example: Debugging a Failing API Integration

**Problem:** Our cron job calls the ORATS API and intermittently returns 403 errors. We've tried rotating the token but it still fails.

**Assumptions identified:**
1. The 403 is caused by an invalid token — BELIEVED (not proven — we haven't checked response body)
2. Token rotation works correctly — GUESSED (we haven't verified the new token is accepted)
3. The endpoint we're calling requires this token — BELIEVED (never re-checked docs)
4. Our token has the right permissions — GUESSED (assumed when it was issued)

**Atomic facts established:**
1. The 403 error body says "endpoint requires Strikes tier subscription" — PROVEN (read from logs)
2. Our ORATS plan is the `datav2/summaries` tier — PROVEN (read from TOOLS.md)
3. We are calling `/datav2/strikes` — PROVEN (read from code)
4. `/datav2/strikes` is marked as 403 Denied in TOOLS.md — PROVEN

**Reconstruction:** The problem is not the token — it's the endpoint tier. The fix is not to rotate the key but to either use a supported endpoint or upgrade the plan. No amount of token rotation will fix a tier permission error.

---

*Write the output to `/tmp/first-principles-<topic>.md` and reference it in the Implementation Readiness Gate.*
