# Step 4: Adversarial Review Gate

**Goal:** Run mandatory adversarial code review before the story can be merged.

---

## EXECUTION

### 1. Construct the Diff

Get all changes made during this story's implementation:

```bash
git diff main..HEAD
```

If not on a branch, diff against the commit recorded at story start.

### 2. Invoke Adversarial Review

Run the 3-Layer Adversarial Review skill (`reviews/adversarial-review/`) with:
- `diff` = the constructed diff
- `spec_file` = the story file (enables Acceptance Auditor layer)

Follow the adversarial review's 4-step process:
1. Construct diff ✓ (done above)
2. Spawn reviewers (3 parallel layers)
3. Triage findings
4. Resolve findings

### 3. Gate Decision

Based on the adversarial review results:

- **Zero Critical findings + all High findings resolved** → ✅ PASS — story is ready for merge
- **Any unresolved Critical findings** → ❌ BLOCKED — must fix before merge
- **Unresolved High findings** → ⚠️ CONDITIONAL — document rationale for deferral, get explicit approval

### 4. Update Story Status

- If PASS: Update story status to `"review"` (ready for human review / merge)
- If BLOCKED: Update story status to `"blocked-review"` with list of blocking findings
- If CONDITIONAL: Update story status to `"review-conditional"` with documented deferrals

### 5. Final Output

```markdown
## Adversarial Gate Result: {PASS / BLOCKED / CONDITIONAL}

**Story:** {title}
**Review Layers:** {completed layers}
**Findings:** {total} ({n} Critical, {n} High, {n} Medium, {n} Low)
**Resolved:** {n} | **Deferred:** {n} | **Dismissed:** {n}

**Recommendation:** {Ready for merge / Fix required / Merge with documented debt}
```

---

## HALT CONDITIONS

- **Adversarial review completely fails (all layers):** HALT — "Cannot pass gate without review. Request human code review."
- **Critical findings and agent cannot fix them:** HALT — "Blocked by Critical findings: {list}. Need human intervention."

## COMPLETION

Story implementation cycle is complete. The branch is either:
1. Ready for PR and merge
2. Blocked and needs fixes (loop back to Step 2)
3. Conditionally ready with documented technical debt
