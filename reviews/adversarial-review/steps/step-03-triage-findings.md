# Step 3: Triage Findings

**Goal:** Deduplicate, classify severity, and present a unified findings report.

---

## EXECUTION

### 1. Collect All Findings

Gather outputs from all completed layers:
- Blind Hunter → markdown list
- Edge Case Hunter → JSON array
- Acceptance Auditor → markdown list (if run)

Note which layers completed and which are in `{failed_layers}`.

### 2. Normalize Format

Convert all findings to a unified format:

| Field | Description |
|-------|-------------|
| **ID** | Sequential: F1, F2, F3... |
| **Title** | One-line description |
| **Severity** | Critical / High / Medium / Low |
| **Source** | Blind / Edge-Case / Acceptance / Multi-Layer |
| **Validity** | Real / Noise / Undecided |
| **Evidence** | Code reference or diff snippet |
| **Fix Hint** | Suggested approach (from edge-case hunter's `guard_snippet` when available) |

### 3. Deduplicate

Cross-reference findings across layers:
- If 2+ layers found the same issue → merge into one finding, mark source as "Multi-Layer", bump confidence
- Same issue = same file + same logical concern (not just same line number)
- Multi-layer findings get severity bumped by one level (e.g., Medium → High)

### 4. Classify Severity

Apply these definitions:
- **Critical:** Data loss, security vulnerability, crash in production path
- **High:** Incorrect behavior for a common case, missing error handling on external calls
- **Medium:** Edge case not handled, inconsistency, potential performance issue
- **Low:** Style, naming, minor improvement, non-blocking

### 5. Order and Present

Sort by: Critical → High → Medium → Low, then by Multi-Layer first within each severity.

Present as a markdown table:

```
| ID | Severity | Source | Title | Validity | Evidence | Fix Hint |
|----|----------|--------|-------|----------|----------|----------|
| F1 | Critical | Multi-Layer | ... | Real | ... | ... |
```

### 6. Summary Stats

Report:
- Total findings: N
- By severity: X Critical, Y High, Z Medium, W Low
- Layers completed: [list]
- Layers failed: [list or "none"]
- Multi-layer detections: N (findings caught by 2+ layers independently)

---

## HALT CONDITIONS

- **Zero total findings across all layers:** HALT — "Zero findings is suspicious. Re-analyze or escalate."
- **All findings classified as Noise:** HALT — "Review produced no actionable findings. Consider re-running with different focus areas."

## NEXT STEP

Present findings to the user/agent. Proceed to `step-04-resolve-findings.md` for resolution decisions.
