# Step 2: Red-Green-Refactor Loop

**Goal:** Implement each task using strict TDD — failing tests first, then implementation, then refactor.

**This step loops for every task/subtask in the story until all are complete.**

---

## EXECUTION (Per Task)

### Phase 1: RED — Write Failing Tests

1. Read the current task/subtask description carefully
2. Write test(s) that verify the expected behavior described in the task
3. **Run the tests — they MUST fail.** If they pass before implementation, the tests are wrong (testing something that already exists or testing nothing meaningful)
4. If tests pass unexpectedly:
   - Investigate why — is the feature already implemented?
   - If already implemented, verify it meets the task spec and mark complete
   - If tests are wrong, rewrite them to actually test the new behavior

**Evidence required:** Capture the failing test output.

### Phase 2: GREEN — Implement Minimal Code

1. Write the MINIMUM code needed to make the failing tests pass
2. Do NOT add features beyond what the current task requires
3. Do NOT optimize or refactor yet — just make it work
4. **Run ALL tests** (not just the new ones) — everything must pass
5. If tests fail:
   - Fix the implementation (not the tests, unless tests had a genuine bug)
   - Re-run until all tests pass
   - If stuck after 3 attempts on the same failure, log the issue in Dev Notes and try a different approach

**Evidence required:** Capture the passing test output (full suite).

### Phase 3: REFACTOR — Improve While Green

1. Review the code you just wrote — is there duplication? Poor naming? Unnecessary complexity?
2. Refactor for clarity and maintainability
3. **Run ALL tests again** — they must still pass after refactoring
4. If tests break during refactor, undo the refactor and try a simpler improvement
5. Ensure code follows project conventions (from Dev Notes / project context)

**Evidence required:** Capture passing test output after refactor.

### Phase 4: MARK COMPLETE

**Only after ALL three phases succeed:**

1. Mark the task checkbox `[x]` in the story file
2. Update the File List with all new/modified/deleted files
3. Add a brief note to Dev Notes: what was implemented, what tests were added
4. Check if more tasks remain:
   - **More tasks:** Loop back to Phase 1 for the next task
   - **No more tasks:** Proceed to Step 3

---

## CRITICAL RULES

- **NEVER mark `[x]` without verified passing tests** — this is the #1 quality rule
- **NEVER skip the RED phase** — if you can't write a failing test first, the task spec is unclear (HALT and ask)
- **NEVER proceed to next task with ANY failing tests** — fix everything before moving on
- **Run the FULL test suite, not just new tests** — catch regressions immediately
- **3 consecutive failures on same approach** → change strategy entirely, don't repeat the same fix

## HALT CONDITIONS

- **Task requirements are ambiguous:** HALT — "Task {n} needs clarification: {what's unclear}"
- **3 consecutive failures on same task:** HALT — "Stuck on Task {n}. Tried: {approaches}. Need guidance."
- **New dependency required beyond story scope:** HALT — "Task {n} requires {dependency} not in story spec"
- **Cannot write a meaningful failing test:** HALT — "Task {n} spec is too vague to test"

## NEXT STEP

When all tasks are `[x]`, proceed to `step-03-validate-complete.md`.
