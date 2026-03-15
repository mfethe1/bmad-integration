# Step 4: Resolve Findings

**Goal:** User or agent decides which findings to fix, defer, or dismiss.

---

## EXECUTION

### 1. Present Resolution Options

For the triaged findings from Step 3, present these options:

- **Fix All** — Address every finding (Critical + High + Medium + Low)
- **Fix Critical + High Only** — Address only Critical and High severity findings; defer Medium/Low
- **Cherry Pick** — User selects specific finding IDs to fix (e.g., "Fix F1, F3, F7")
- **Dismiss with Reason** — Mark specific findings as intentional/not-applicable with documented rationale

### 2. Record Decisions

For each finding, record the resolution:

| ID | Decision | Reason (if deferred/dismissed) |
|----|----------|-------------------------------|
| F1 | Fix | — |
| F2 | Defer | Low priority, tracked in backlog |
| F3 | Dismiss | Intentional behavior per spec section 4.2 |

### 3. Execute Fixes

For findings marked "Fix":
- Fix in severity order (Critical first)
- After each fix, run relevant tests to verify no regression
- Mark the finding as resolved

### 4. Re-Review Gate (Optional)

If more than 5 findings were fixed:
- Run a targeted re-review on ONLY the fix commits
- This catches fixes that introduce new issues
- Limit to Blind Hunter only (fast pass) unless fixes were complex

### 5. Final Report

Produce a summary:

```markdown
## Adversarial Review Summary

**Review Date:** {date}
**Diff:** {baseline} → {current}
**Layers Run:** {list}

### Results
- Findings: {total} (Critical: {n}, High: {n}, Medium: {n}, Low: {n})
- Fixed: {n}
- Deferred: {n}  
- Dismissed: {n}

### Deferred Items
- F{n}: {title} — Reason: {reason}

### Dismissed Items  
- F{n}: {title} — Reason: {reason}
```

Save this report adjacent to the reviewed code or in the project's review artifacts directory.

---

## HALT CONDITIONS

- **Critical findings dismissed without documented rationale:** HALT — "Critical findings require explicit justification to dismiss."
- **No resolution selected after presenting options:** HALT — "Cannot proceed without resolution decisions."

## COMPLETION

Review cycle is complete. The code is either:
1. **Clean** — all findings resolved → ready for merge
2. **Partially resolved** — deferred items tracked → ready for merge with known debt
3. **Blocked** — unresolved Critical findings → NOT ready for merge
