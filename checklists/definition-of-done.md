# Master Definition of Done
**Version:** 1.0  
**Owner:** Lenny (QA/Quality)  
**Replaces:** 6-checkbox Quality Gates in AGENTS.md

A task is DONE only when the applicable checklist below passes completely.  
Mark each item `[x]` with a brief evidence note. Unmarked items = task is NOT done.

---

## How to Use

1. Identify task type (Feature / Bug Fix / Infrastructure / Documentation / Cron Job)
2. Load the corresponding section below
3. Work through each item — check it only when evidence exists
4. Complete the Final Validation Output block
5. Update `status/<agent>.json` and `BACKLOG.md`

---

## 1. Feature Implementation DoD

*Use for: new functionality, new API endpoints, new UI components, new agent skills*

### Requirements & Scope
- [ ] All acceptance criteria from the task/story are implemented — each AC has a corresponding code path
- [ ] Edge cases identified during implementation are handled (or explicitly deferred with a tracked issue)
- [ ] No scope creep — only what was specified was built (undocumented extras noted and justified)
- [ ] Product constraints (perf thresholds, SLA, rate limits) are enforced, not just the happy path
- [ ] Feature is behind a feature flag if it changes user-facing behavior significantly

### Implementation Quality
- [ ] Code follows project conventions (naming, structure, patterns from `project-context.md`)
- [ ] No `any` in TypeScript without documented justification
- [ ] All Python functions have type hints
- [ ] No hardcoded secrets, API keys, or environment-specific values in code
- [ ] No dead code or unused imports left in the diff
- [ ] No TODO/FIXME comments without a linked issue number
- [ ] Error handling is present on all external calls (API, DB, filesystem)
- [ ] All async operations properly awaited; no floating promises

### Testing
- [ ] Unit tests written for all new functions (≥1 test per function, covering happy + error path)
- [ ] Integration test covers the full critical path end-to-end
- [ ] All existing tests pass — no regressions
- [ ] Test coverage did not decrease
- [ ] Tests are deterministic (no time-based flakiness, no random seeds without fixture)

### Security
- [ ] Authentication enforced on all new endpoints
- [ ] Authorization checked (not just "logged in" but "can this user do this")
- [ ] Inputs validated and sanitized
- [ ] No new CVEs introduced via new dependencies

### Documentation
- [ ] Public API endpoints documented (docstring or OpenAPI schema)
- [ ] Complex logic has "why" comments, not just "what" comments
- [ ] README updated if environment setup or architecture changed
- [ ] CHANGELOG entry added

### Deployment Readiness
- [ ] New environment variables documented and added to all target environments
- [ ] Database migrations tested (up AND down)
- [ ] No breaking changes to existing API consumers (or all consumers updated)
- [ ] Rollback plan identified for significant changes

### Completion Evidence
- [ ] `status/<agent>.json` updated with task completion
- [ ] `BACKLOG.md` updated (task moved to done or removed)
- [ ] Code committed and pushed on feature branch
- [ ] PR opened with description linking to task

### Final Validation Output
```
Feature DoD: [ PASS | FAIL ]
Score: N/N items passed
Blocked items: [list any failed checks]
Evidence summary: [test results, PR link, env status]
```

---

## 2. Bug Fix DoD

*Use for: defect fixes, hotfixes, regression patches*

### Root Cause
- [ ] Root cause identified and documented (not just the symptom)
- [ ] Root cause is addressed, not just the surface manifestation
- [ ] Related code paths with the same root cause pattern checked and fixed if needed
- [ ] Postmortem note written if this was a production incident (even one sentence)

### Fix Quality
- [ ] Fix is minimal — changes only what is needed to address the root cause
- [ ] Fix does not introduce new behavior changes beyond the defect correction
- [ ] No quick hacks that defer the problem (if deferral is necessary, create a tracked issue)
- [ ] Code follows project conventions
- [ ] Error messages improved if the bug was hard to debug

### Reproduction & Verification
- [ ] Bug was reproduced before fix (proof: log output, test failure, screenshot)
- [ ] Bug is no longer reproducible after fix (proof: test pass, manual verification)
- [ ] Fix verified in the environment where the bug was reported
- [ ] Edge cases that triggered the bug tested explicitly

### Testing
- [ ] Regression test added that would have caught this bug before it shipped
- [ ] All existing tests pass
- [ ] Test covers the exact scenario from the bug report
- [ ] Related tests (same module/feature) also pass

### Documentation
- [ ] Bug ticket/issue updated with resolution
- [ ] CHANGELOG entry added for user-visible bug fixes
- [ ] If the bug revealed a systemic issue, it's captured in `FAILURES.md`

### Deployment Readiness
- [ ] Hotfix branch follows naming convention (`hotfix/<agent>/<description>`)
- [ ] If deployed to production, deployment verified (not just "merged")
- [ ] Monitoring/alerting checked post-deployment to confirm fix held

### Completion Evidence
- [ ] `status/<agent>.json` updated
- [ ] `BACKLOG.md` updated
- [ ] PR merged or in review

### Final Validation Output
```
Bug Fix DoD: [ PASS | FAIL ]
Score: N/N items passed
Root cause confirmed: [yes/no]
Regression test added: [yes/no — if no, justification]
Production verification: [status]
```

---

## 3. Infrastructure Change DoD

*Use for: cron jobs, server config, Railway deploys, Cloudflare changes, Docker changes, secret rotations, Tailscale changes*

### Pre-Change
- [ ] Change documented before execution (what changes, why, expected outcome)
- [ ] Rollback procedure defined and tested/verified in advance
- [ ] Dependencies checked (does this change affect other services?)
- [ ] Change window identified (avoid peak traffic if possible)
- [ ] Team notified if change has user impact

### Change Execution
- [ ] Change applied exactly as documented (no improvised deviations)
- [ ] All target environments updated (not just one env forgotten)
- [ ] Secrets rotated properly — old secrets invalidated, new secrets propagated
- [ ] Configuration files committed (no undocumented changes in production only)

### Verification
- [ ] Health check passed after change (endpoint, ping, service status)
- [ ] Dependent services verified still functioning
- [ ] Logs checked for unexpected errors post-change
- [ ] Monitoring dashboards reviewed (no spikes, no new alerts)
- [ ] Performance baseline unchanged (no latency regression)

### Security
- [ ] No new exposed ports or surfaces
- [ ] Least-privilege principle maintained for new service accounts/roles
- [ ] Secret values not logged or exposed in deployment output

### Documentation
- [ ] Change logged in infrastructure change log (or CHANGELOG)
- [ ] Architecture diagrams updated if topology changed
- [ ] Runbook updated if operational procedures changed
- [ ] New environment variables documented in team secrets manager

### Completion Evidence
- [ ] `status/<agent>.json` updated
- [ ] `BACKLOG.md` updated
- [ ] Verification evidence captured (screenshot, log snippet, health check output)

### Final Validation Output
```
Infrastructure Change DoD: [ PASS | FAIL ]
Score: N/N items passed
Health check status: [PASS/FAIL]
Rollback verified: [yes/no]
Dependent services status: [all green / issues noted]
```

---

## 4. Documentation DoD

*Use for: README updates, SOUL.md changes, SKILL.md files, runbooks, API docs, architecture docs*

### Content Quality
- [ ] Accuracy verified — content matches actual current behavior (not aspirational or outdated)
- [ ] All code examples tested and working (copy-paste should work)
- [ ] All links verified as working (no 404s)
- [ ] Technical claims verified against source of truth (code, config, logs)
- [ ] Audience is clear — docs written at the right level for intended reader

### Completeness
- [ ] All prerequisites listed (tools, credentials, access needed)
- [ ] Step-by-step instructions are complete — no assumed knowledge gaps
- [ ] Error scenarios documented (what to do if X fails)
- [ ] Version/date noted where content may become stale
- [ ] Related docs cross-referenced

### Structure & Style
- [ ] Consistent with existing doc style in the same repo/directory
- [ ] Headers create navigable structure (H1 → H2 → H3, not skipped)
- [ ] Code blocks use appropriate language hints for syntax highlighting
- [ ] No walls of text — complex explanations broken into lists or tables

### Agent-Specific (if applicable)
- [ ] SOUL.md changes reviewed for unintended behavior modifications
- [ ] AGENTS.md changes reviewed against all 4 agents — no agent excluded from updates
- [ ] SKILL.md changes tested (skill loads and operates as documented)
- [ ] Memory instructions are clear and will produce consistent memU writes

### Completion Evidence
- [ ] `status/<agent>.json` updated
- [ ] `BACKLOG.md` updated
- [ ] Doc committed and pushed

### Final Validation Output
```
Documentation DoD: [ PASS | FAIL ]
Score: N/N items passed
Code examples tested: [yes/no]
Links verified: [yes/no]
Accuracy verified against: [source of truth listed]
```

---

## 5. Cron Job DoD

*Use for: new cron schedules, cron job modifications, heartbeat changes, recurring automation*

### Design
- [ ] Job purpose is documented (one sentence: "This job does X every Y because Z")
- [ ] Schedule is justified (why this frequency? is there a more event-driven trigger?)
- [ ] Job is idempotent — running twice produces the same result as running once
- [ ] Job has a timeout set (`timeoutSeconds` configured — never unbounded)
- [ ] NOOP policy defined (what to do if there's nothing to act on — skip silently, not alert-spam)

### Implementation
- [ ] Job reads coordination root with fallback (Edrive → local → degraded)
- [ ] Job does not call external APIs without handling 429/rate-limit gracefully
- [ ] Job uses explicit message tool for notifications, not `delivery.mode=announce`
- [ ] Job handles missing files gracefully (no crash if optional files don't exist)
- [ ] Secrets accessed via env var or secrets file, never hardcoded
- [ ] Job writes intermediate results to files (not only in volatile context)

### Testing
- [ ] Job tested manually before scheduling (one dry run with output verified)
- [ ] Edge case tested: what happens when job runs during Edrive outage?
- [ ] Edge case tested: what happens when job runs with empty/missing data?
- [ ] NOOP output is clean and not spammy when there's nothing to do

### Monitoring
- [ ] Job output format is parseable (consistent structure for log auditing)
- [ ] Failure mode documented — what happens if job errors? Who is alerted?
- [ ] Job added to cron health scan checklist
- [ ] 3-consecutive-failure policy configured (disable + alert after 3 failures)

### Schedule & Coordination
- [ ] Schedule avoids peak usage windows (unless intentional)
- [ ] Schedule conflicts with other cron jobs reviewed (no overlapping heavy jobs)
- [ ] Job registered in coordination root's job registry (if applicable)
- [ ] Weekend/overnight throttle applied if appropriate (per NOOP Hard Limits in AGENTS.md)

### Documentation
- [ ] Cron entry commented with: purpose, owner, schedule rationale, timeout
- [ ] BACKLOG updated with job as recurring operational item

### Completion Evidence
- [ ] `status/<agent>.json` updated
- [ ] Test run output captured as evidence
- [ ] Job schedule verified in cron tab

### Final Validation Output
```
Cron Job DoD: [ PASS | FAIL ]
Score: N/N items passed
Idempotency verified: [yes/no]
Timeout configured: [Xs]
Test run output: [attached/inline]
Failure policy: [documented/not documented]
```

---

*Last updated: 2026-03-15 | Maintained by Lenny (QA)*
