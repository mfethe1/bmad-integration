# Red Team
**Method:** Adversarial attack on your own plan  
**Origin:** Military wargaming / penetration testing  
**Best for:** Security review, architecture validation, agent prompt design, any plan with stated guarantees

---

## What It Is

A red team exercise assigns you — or a subagent — the role of **adversary**. Your job is to defeat the plan, break the system, exploit the design, or find the argument that invalidates the approach.

The red team is not a critic. Critics point out flaws. The red team **actively tries to make the plan fail** — finding the exploit, not just noting the vulnerability.

Key constraint: **the red team must be honest.** "This is actually fine" is an acceptable finding if the adversary genuinely cannot break it. But the red team must try hard before conceding.

---

## When to Run It

- Before shipping any system that has stated security or reliability guarantees
- After designing an agent workflow or multi-step automation
- Before presenting an architecture to the team
- When your plan feels airtight (that's when it usually isn't)
- When evaluating another agent's proposed approach

---

## Template

```markdown
# Red Team Exercise: [Plan/System Name]
**Date:** YYYY-MM-DD
**Target:** [What you're attacking — be specific]
**Attacker persona:** [Security adversary | Internal bad actor | Murphy's Law | Naive user | Competing agent]
**Author:** [Agent name]

---

## The Plan You're Attacking

[Copy the plan, design, or system description here. The red team attacks THIS specific thing, not a generalized version.]

---

## Phase 1: Map the Attack Surface

Before attacking, map what can be attacked. Every plan has:
- **Inputs** (what data/triggers enter the system)
- **Assumptions** (what must be true for the plan to work)  
- **Critical paths** (the paths where failure has the highest impact)
- **Integration points** (where this system touches others)
- **Trust boundaries** (where something trusted could be compromised)

| Attack Surface | Description | Why Valuable to Attack |
|---------------|-------------|----------------------|
| | | |
| | | |
| | | |

---

## Phase 2: Execute the Attacks

For each attack surface, run the most effective attack you can imagine.

### Attack 1: [Name the attack]
**Target:** [Which attack surface]
**Method:** [How you attack it]
**Exploit:**
```
[Describe the specific sequence of actions, inputs, or conditions that trigger the failure]
```
**Result:** [What breaks, what data is compromised, what behavior is wrong]
**Severity:** [Critical | Major | Minor]
**Likelihood:** [High | Medium | Low]
**Evidence that this works:** [Why you believe this attack succeeds]

### Attack 2: [Name the attack]
[... same format ...]

### Attack 3: [Name the attack]
[... same format ...]

*(Continue until you've attempted at least 5 attacks or exhausted all attack surfaces)*

---

## Phase 3: Chain Attacks

The most dangerous failures often require combining multiple vulnerabilities.

**Attack Chain 1:** [Step A] → [Step B] → [Step C] → [Failure]
**Chain severity:** [Critical | Major]
**Chain description:** [Walk through how each step enables the next]

---

## Phase 4: Red Team Verdict

After exhausting attack options:

| Attack | Result | Severity |
|--------|--------|----------|
| Attack 1 | [SUCCEEDED / BLOCKED / PARTIAL] | |
| Attack 2 | [SUCCEEDED / BLOCKED / PARTIAL] | |
| Attack 3 | [SUCCEEDED / BLOCKED / PARTIAL] | |

**Weakest points in the plan:**
1. [Specific vulnerability with evidence]
2. [Specific vulnerability with evidence]
3. [Specific vulnerability with evidence]

**Recommendation:**
- [ ] Plan has critical vulnerabilities — do not proceed without addressing these
- [ ] Plan has significant vulnerabilities — proceed with explicit mitigations
- [ ] Plan has minor vulnerabilities — proceed with monitoring in place
- [ ] Plan successfully resisted all attacks — proceed with confidence

---

## Phase 5: Defense Patches

For each successful attack, design a defense:

### Defense for Attack 1: [attack name]
**Mitigation:** [specific change to the plan that blocks this attack]
**Tradeoff:** [what does this mitigation cost in complexity, performance, or behavior?]
**Verification:** [how will you test that the mitigation works?]

---

## Output

```
Red Team Exercise Complete

Attacks attempted: N
Successful attacks: N (list)
Critical vulnerabilities: N (list)
Plan status: [HALT — critical flaws | REVISE — significant flaws | PROCEED — minor/no flaws]

Required plan changes before proceeding:
1. [change]
2. [change]
```
```

---

## Attacker Personas

Choose the persona that's most relevant to your plan:

| Persona | Goal | Attacks focus on |
|---------|------|-----------------|
| **External adversary** | Breach the system | Input manipulation, auth bypass, data exfiltration |
| **Internal bad actor** | Abuse privileges | Escalation, data exfiltration via legitimate access |
| **Murphy's Law** | Find the unlucky failure | Race conditions, timing, network flakiness |
| **Naive user** | Break it by doing it wrong | Unexpected inputs, wrong order of operations |
| **Competing agent** | Subvert coordination | Stale files, conflicting writes, message corruption |
| **Time** | Rot the system | Credential expiry, schema drift, dependency deprecation |

---

## Example: Agent Cron Job Security Review

**Target:** Cron job that reads from BACKLOG.md, claims unowned tasks, and writes status updates.

**Attack surface mapped:**
- Input: BACKLOG.md file contents (can be modified by any process with file access)
- Assumption: file contents are valid markdown from trusted agents
- Critical path: task claiming logic (prevents duplicate work)
- Trust boundary: file system (assumed local, trusted)

**Attack 1: BACKLOG.md injection**
Method: Write a specially crafted task to BACKLOG.md that tricks the agent into running an arbitrary command embedded in the task description.
Result: If the agent passes task descriptions to `exec()` without sanitization, arbitrary code execution.
Severity: Critical.
Defense: Never execute task description strings directly. Treat all file content as untrusted data.

**Attack 2: Race condition on task claiming**
Method: Two agents simultaneously read BACKLOG.md, both see the same unowned task, both claim it.
Result: Duplicate work, conflicting status updates, potentially two partial implementations.
Severity: Major.
Defense: Use file locking or a dedicated claim file with atomic write semantics.

---

*Write red team output to `/tmp/redteam-<plan>.md`. Critical findings must be resolved before proceeding.*
