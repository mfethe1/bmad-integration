# Elicitation Methods
**Owner:** Lenny (QA/Quality)  
**Version:** 1.0

Structured reasoning methods for going deeper than first-pass analysis.  
Use these when a task is high-stakes, complex, or when your first answer feels too easy.

---

## What Are Elicitation Methods?

Elicitation methods are **structured second-pass reasoning frameworks**. They force you to approach a problem from a non-default angle — breaking confirmation bias, surfacing blind spots, and pressure-testing assumptions before they become production failures.

The key insight: **your first answer is often wrong in an interesting way**. Elicitation methods are designed to find that wrongness before it ships.

---

## When to Use Them

| Situation | Recommended Method |
|-----------|-------------------|
| Planning a complex feature or system change | Pre-Mortem + First Principles |
| Reviewing your own implementation plan | Red Team + Inversion |
| Stuck on a problem with an obvious-seeming answer | First Principles |
| About to ship something high-stakes | Pre-Mortem |
| Evaluating an approach someone else proposed | Red Team |
| Trying to prevent a category of failure | Inversion |
| All of the above | Run all 4 in sequence (15-20 min) |

---

## Available Methods

### 1. [Pre-Mortem](./pre-mortem.md)
**Template: "Imagine this failed. Why?"**

Travel to a future where the plan failed. Work backward to identify the causes. Most powerful for plans with many moving parts where failure modes aren't obvious until something breaks.

**Best for:** Launch planning, infrastructure changes, multi-step workflows, cron job design.

---

### 2. [First Principles](./first-principles.md)
**Template: Strip assumptions, rebuild from ground truth.**

Decompose the problem to its atomic facts. Discard inherited assumptions. Reconstruct the solution from fundamentals. Used by Feynman, Musk, Bohr. Effective when the conventional approach is wrong but "feels right" due to momentum.

**Best for:** Debugging mysterious failures, designing novel solutions, escaping local optima, questioning "we've always done it this way."

---

### 3. [Red Team](./red-team.md)
**Template: Attack your own plan. Find the weakest points.**

Assign yourself the role of adversary. Your job is to defeat the plan. Find exploits, failure modes, and design weaknesses before a real adversary (or Murphy's Law) does.

**Best for:** Security review, architecture validation, agent prompt design, any plan with stated guarantees.

---

### 4. [Inversion](./inversion.md)
**Template: Instead of "how to succeed", ask "how could this fail?"**

Charlie Munger's mental model. Instead of forward-planning success, backward-plan failure — then avoid those conditions. Often reveals obvious-in-retrospect failure modes that forward-planning misses.

**Best for:** Goal setting, system design, process improvement, any task where you're trying to guarantee an outcome.

---

## Combining Methods

For maximum coverage on critical work, run in this order:

```
1. Pre-Mortem    → surfaces the failure scenarios you fear most
2. Inversion     → maps ALL the ways to fail, systematically
3. Red Team      → attacks the specific plan you've written
4. First Principles → rebuilds any component that failed the attack
```

**Time budget:**
- Quick pass: 5 min per method × 4 = 20 min
- Deep pass: 15-20 min per method = 60-80 min
- For high-stakes changes: deep pass is required

---

## Integration with Adversarial Review

These elicitation methods complement the [Adversarial Review Protocol](../reviews/adversarial-review-protocol.md).

- Use elicitation methods **before implementation** (planning phase)
- Use adversarial review **after implementation** (review phase)

The pre-mortem and red team findings should directly inform what the Edge Case Hunter and Blind Hunter look for during code review.

---

## Format

Every elicitation method produces a structured output that should be:
1. Written to a file (not kept only in volatile context)
2. Referenced in the Implementation Readiness Gate
3. Shared with relevant agents via agent-inboxes if findings affect shared systems

---

*These methods are adapted from cognitive science, military wargaming, and engineering risk analysis — applied to agent-assisted software delivery.*
