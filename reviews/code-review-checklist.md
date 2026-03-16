# Code Review Checklist
**Version:** 1.0  
**Owner:** Lenny (QA/Quality)  
**Stack:** Python/FastAPI · TypeScript/Next.js · PostgreSQL · Railway · OpenClaw agents

Run this checklist for every PR. Check each item explicitly — do not skim.  
Items marked 🔴 are blocking; items marked 🟡 are major; items marked 🔵 are minor.

---

## Section 1: Logic Correctness

- [ ] 🔴 All conditional branches are covered and correct (no inverted conditions)
- [ ] 🔴 Return values are used — not silently discarded where they matter
- [ ] 🔴 Boolean logic is correct (check De Morgan, operator precedence, short-circuit evaluation)
- [ ] 🔴 Loop termination is guaranteed (no infinite loops, correct exit conditions)
- [ ] 🔴 Off-by-one errors checked in all array/slice/range operations
- [ ] 🔴 Numeric overflow/underflow considered for financial or count fields
- [ ] 🟡 No unreachable code or dead branches
- [ ] 🟡 Function side effects are documented and expected
- [ ] 🟡 Recursive functions have verified base cases and no stack overflow risk
- [ ] 🔵 Algorithm complexity is appropriate for expected input scale
- [ ] 🔵 No unnecessary re-computation inside loops

---

## Section 2: Error Handling

- [ ] 🔴 All external calls (API, DB, filesystem) wrapped in error handling
- [ ] 🔴 Errors are propagated or handled — not silently swallowed
- [ ] 🔴 `try/except` or `try/catch` blocks catch specific exceptions, not bare `Exception`/`Error`
- [ ] 🔴 Failed operations trigger rollback where needed (DB transactions, multi-step ops)
- [ ] 🔴 Error messages include enough context for debugging (not just "error occurred")
- [ ] 🟡 HTTP status codes in API responses match the error type (4xx vs 5xx)
- [ ] 🟡 User-facing error messages don't leak internal stack traces or secrets
- [ ] 🟡 Retry logic (if present) is idempotent — retries can't double-apply side effects
- [ ] 🟡 Timeouts are set on all external calls (no indefinite blocking)
- [ ] 🔵 Fallback behavior is defined when non-critical dependencies fail
- [ ] 🔵 Errors are logged at appropriate level (error vs. warn vs. info)

---

## Section 3: Security

- [ ] 🔴 No secrets, API keys, tokens, or passwords in code, logs, or comments
- [ ] 🔴 All user inputs are validated and sanitized before use (SQL injection, XSS, path traversal)
- [ ] 🔴 SQL queries use parameterized statements or ORM — never string concatenation
- [ ] 🔴 Authentication is enforced on all protected routes/endpoints
- [ ] 🔴 Authorization is checked — not just "are you logged in" but "can YOU do this"
- [ ] 🔴 File uploads validated: type, size, name sanitization
- [ ] 🔴 Sensitive data (PII, credentials) not logged
- [ ] 🟡 Rate limiting considered for public endpoints
- [ ] 🟡 CORS policy is restrictive, not wildcard `*` on production
- [ ] 🟡 JWT/session tokens validated properly (expiry, signature, claims)
- [ ] 🟡 Dependencies checked for known CVEs (if adding new packages)
- [ ] 🟡 Environment-specific configs not accidentally exposed in responses
- [ ] 🔵 Least-privilege principle applied to DB roles and service accounts
- [ ] 🔵 Audit log entries created for sensitive actions (admin ops, auth events)

---

## Section 4: Performance

- [ ] 🔴 No N+1 query patterns (DB queries inside loops without batching)
- [ ] 🔴 Large result sets are paginated — not fetched in entirety
- [ ] 🟡 Indexes exist for all query predicates on hot paths
- [ ] 🟡 Blocking I/O not called in async context without proper threading
- [ ] 🟡 Cache invalidation logic is correct (no stale reads after writes)
- [ ] 🟡 Memory allocations in hot paths are bounded and reasonable
- [ ] 🟡 External service calls that can be parallelized are not serialized
- [ ] 🔵 Bundle size impact assessed for frontend changes (no unnecessary imports)
- [ ] 🔵 Images/assets are optimized and not inlined as base64 unnecessarily
- [ ] 🔵 No synchronous sleep/delay in production paths

---

## Section 5: Test Coverage

- [ ] 🔴 All new functions have at least one unit test
- [ ] 🔴 All acceptance criteria from the task/story have a corresponding test
- [ ] 🔴 Existing tests pass — no regressions introduced
- [ ] 🟡 Happy path AND at least one error/edge case tested per function
- [ ] 🟡 Tests are deterministic — no random behavior, no time-dependent flakiness
- [ ] 🟡 Mocks/stubs are realistic — not masking the actual behavior being tested
- [ ] 🟡 Integration tests cover the critical path (auth → action → DB → response)
- [ ] 🔵 Test names describe the scenario and expected outcome
- [ ] 🔵 Test data factories used instead of raw hardcoded objects
- [ ] 🔵 Coverage delta reviewed — did coverage go up or down?

---

## Section 6: Documentation

- [ ] 🔴 Public API endpoints documented (docstring or OpenAPI annotation)
- [ ] 🔴 Complex logic has inline comments explaining the "why", not the "what"
- [ ] 🟡 README updated if setup steps, env vars, or architecture changed
- [ ] 🟡 CHANGELOG entry added for user-visible changes
- [ ] 🟡 Function/method docstrings present for all public interfaces
- [ ] 🟡 Deprecated functions/APIs marked with deprecation notice and migration path
- [ ] 🔵 Type hints present on all Python functions (in/out)
- [ ] 🔵 TypeScript types are explicit — no `any` without documented justification
- [ ] 🔵 TODO/FIXME comments reference a tracked issue, not just left floating

---

## Section 7: Accessibility

*(Apply to frontend/UI changes only)*

- [ ] 🔴 Interactive elements (buttons, links, forms) have accessible labels (aria-label or visible text)
- [ ] 🔴 Images have descriptive alt text (or `alt=""` for decorative)
- [ ] 🔴 Form inputs have associated `<label>` elements
- [ ] 🟡 Keyboard navigation works for all interactive components
- [ ] 🟡 Focus management is correct after dynamic content changes (modals, alerts)
- [ ] 🟡 Color is not the sole means of conveying information
- [ ] 🟡 Contrast ratio meets WCAG 2.1 AA (4.5:1 for normal text, 3:1 for large)
- [ ] 🔵 ARIA roles and attributes are used correctly (not overriding semantic HTML unnecessarily)
- [ ] 🔵 Error messages are announced to screen readers (aria-live or role="alert")

---

## Section 8: Breaking Changes

- [ ] 🔴 Database migrations: are they reversible? Is a down migration provided?
- [ ] 🔴 API response shape changes: are consumers updated or backward-compatible?
- [ ] 🔴 Removed or renamed public functions/endpoints: deprecation path or version bump
- [ ] 🔴 Environment variable renames/additions: documented, deployed to all envs before code change
- [ ] 🔴 Agent protocol changes (OpenClaw skills, message formats): all consuming agents updated
- [ ] 🟡 Config file format changes: migration guide provided
- [ ] 🟡 Third-party dependency major version bump: breaking changes reviewed
- [ ] 🟡 Cron job schedule or payload changes: existing in-flight jobs considered
- [ ] 🔵 Feature flags needed for gradual rollout of significant behavior changes?
- [ ] 🔵 Rollback plan documented for high-risk changes

---

## Final Verdict

```
Code Review Result: [ APPROVE | APPROVE WITH MINOR FIXES | REQUEST CHANGES | BLOCK ]

🔴 Blocking issues: N
🟡 Major issues: N
🔵 Minor issues: N

Required before merge:
- [ ] [list blocking fixes]

Recommended (non-blocking):
- [ ] [list major/minor improvements]
```

**Reviewer signature:** [Agent name + date]  
**Review type:** [Blind Hunter | Edge Case Hunter | Acceptance Auditor | Manual]
