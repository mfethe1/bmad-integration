# Step 3: Validate Completion (Definition of Done)

**Goal:** Verify the story meets all completion criteria before it can proceed to review.

---

## EXECUTION

### 1. Task Completeness Scan

Re-read the entire story file and verify:
- [ ] ALL tasks and subtasks are marked `[x]`
- [ ] No tasks were skipped or left partially done

If any task is incomplete → go back to `step-02-red-green-refactor.md` for that task.

### 2. Acceptance Criteria Verification

For EACH acceptance criterion in the story:
- [ ] Verify it is satisfied by the implementation
- [ ] Identify which test(s) prove it works
- [ ] If quantitative thresholds exist (performance, coverage), verify with actual measurements

If any AC is NOT met → identify which task should have covered it, add a new task, and go back to Step 2.

### 3. Full Test Suite

- [ ] Run the complete test suite (not just new tests)
- [ ] ALL tests pass — zero failures, zero skips
- [ ] Capture the test output as evidence

If any tests fail → fix before proceeding.

### 4. Code Quality Checks

- [ ] Linting passes (if configured in project)
- [ ] Type checking passes (if TypeScript/typed language)
- [ ] No console.log / debug statements left in production code
- [ ] No TODO/FIXME/HACK comments added without a tracking reference

### 5. File List Audit

- [ ] File List in the story includes ALL new/modified/deleted files
- [ ] Paths are relative to repo root
- [ ] No unexpected files were modified outside the story's scope

### 6. Documentation

- [ ] Dev Notes updated with implementation decisions
- [ ] Any new APIs or interfaces are documented
- [ ] README updated if user-facing behavior changed

---

## COMPLETION SUMMARY

Produce:

```markdown
## Story Completion Summary

**Story:** {title}
**Status:** Ready for Review

### Checklist
- [x] All {n}/{n} tasks complete
- [x] All {n}/{n} acceptance criteria verified  
- [x] Full test suite: {n} tests, {n} passing, 0 failing
- [x] Linting: {pass/fail/not configured}
- [x] Type checking: {pass/fail/not configured}
- [x] File list: {n} files changed

### Files Changed
{list of files}

### Tests Added
{list of new test files/cases}
```

---

## HALT CONDITIONS

- **Any AC not satisfied:** HALT — "AC '{criteria}' is not met. Need additional implementation."
- **Test failures:** HALT — "Cannot complete with failing tests."
- **Files modified outside story scope:** HALT — "Unexpected changes in {files}. Verify these are intentional."

## NEXT STEP

Proceed to `step-04-adversarial-gate.md` for mandatory code review before merge.
