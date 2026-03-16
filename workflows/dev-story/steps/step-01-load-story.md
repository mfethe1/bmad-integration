# Step 1: Load Story

**Goal:** Read and parse the complete story/spec before any implementation begins.

---

## EXECUTION

### 1. Locate Story File

- If a story path is provided explicitly, use it directly
- If a BACKLOG.md entry references the task, find the associated spec/story file
- If neither exists, ask the user for the story file path

### 2. Read COMPLETE Story File

**Read the entire file before taking any action.** Do not skim. Do not start implementing after reading the first task.

Parse and extract:
- **Story/Task Description** — what is being built
- **Acceptance Criteria** — measurable conditions for completion
- **Tasks/Subtasks** — ordered list of implementation steps (checkboxes)
- **Dev Notes** — architecture requirements, previous learnings, constraints
- **File List** — files expected to be changed
- **Dependencies** — what must exist before this story can start

### 3. Identify First Incomplete Task

Scan all tasks/subtasks for the first unchecked `[ ]` item. This is where implementation begins.

- If ALL tasks are already checked `[x]` → skip to Step 3 (validate-complete)
- If no tasks exist → HALT: "Story has no implementation tasks defined"

### 4. Load Project Context

Read relevant project files for coding standards and patterns:
- `AGENTS.md` or `.openclaw/AGENTS.md` in the project repo (if exists)
- Test framework configuration (package.json, pytest.ini, etc.)
- Existing test examples for style/pattern reference

### 5. Confirm Readiness

Output a brief confirmation:
```
✅ Story loaded: {story_title}
   Tasks: {completed}/{total} complete
   Starting at: Task {n} — {task_description}
   Test framework: {detected_framework}
```

---

## HALT CONDITIONS

- **Story file not found or inaccessible:** HALT — "Cannot implement without a story file."
- **Story has no tasks defined:** HALT — "Story needs implementation tasks before development can start."
- **Acceptance criteria missing:** HALT — "Cannot verify completion without acceptance criteria."

## NEXT STEP

Proceed to `step-02-red-green-refactor.md` for the TDD implementation loop.
