# Step 1: Construct Diff

**Goal:** Build a complete diff of all changes to review.

---

## EXECUTION

### 1. Determine Diff Source

Check how the diff should be constructed:

- **If a git branch/commit is provided:** Run `git diff <baseline>..<current>` to capture all tracked changes. Also check for new untracked files created during the work and include their full content.
- **If file paths are provided:** Read current state of each file and construct a before/after diff.
- **If a PR number is provided:** Use `gh pr diff <number>` to fetch the diff.

### 2. Capture as `{diff_output}`

Merge all changes into a single `{diff_output}` variable.

**Size guard:** If the diff exceeds 50,000 characters, split into logical chunks (by file or directory) and note that reviewers will process chunks sequentially.

### 3. Determine Review Mode

- If `{spec_file}` was provided and exists → set `{review_mode}` = `"full"` (all 3 layers)
- If no spec file → set `{review_mode}` = `"no-spec"` (layers 1 + 2 only)

### 4. Record Baseline

Store `{baseline_commit}` (git HEAD or "NO_GIT") for later reference.

---

## HALT CONDITIONS

- **Diff is empty:** HALT — "No changes detected. Nothing to review."
- **Git repo not found and no files provided:** HALT — "Cannot construct diff without a source."

## NEXT STEP

With `{diff_output}` and `{review_mode}` ready, proceed to `step-02-spawn-reviewers.md`.
