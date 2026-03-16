# Spec Scope Standards

## The Rule

Every spec/story must represent a **single user-facing goal** and fit within **900–1600 tokens**.

## Why

- Specs over 1600 tokens → agents lose focus, skip requirements, produce partial implementations
- Specs under 900 tokens → insufficient detail, agents make assumptions, output quality drops
- Multi-goal specs → agents do half of each goal well instead of one goal completely

## How to Split

When a request has multiple goals, split into independent shippable deliverables:

### Bad (Multi-Goal)
> "Add user authentication, profile page, and password reset"

### Good (Single-Goal Each)
1. **Story 1:** "Add user login with email/password" (covers: form, validation, session, tests)
2. **Story 2:** "Add user profile page" (covers: route, display, edit, tests) — depends on Story 1
3. **Story 3:** "Add password reset flow" (covers: request, email, token, reset, tests) — depends on Story 1

### Splitting Heuristic

Ask: "Could I ship this to a user without the other parts and they'd get value?"
- Yes → it's a valid independent story
- No → it's a subtask within a larger story, not a separate story

## Validation Checklist

Before an agent starts implementing a spec:

- [ ] Single user-facing goal (not multiple features bundled)
- [ ] 900–1600 tokens (estimate: count words × 1.3)
- [ ] Clear acceptance criteria (testable, not vague)
- [ ] Dependencies explicitly listed
- [ ] Out-of-scope explicitly stated (what this spec does NOT cover)
- [ ] No implementation details in the spec (WHAT not HOW — let the dev agent decide HOW)

## Implementation Leakage

A spec should describe WHAT, not HOW. Signs of implementation leakage:
- Mentioning specific libraries or frameworks (unless it's a constraint)
- Describing database schemas
- Including code snippets
- Specifying API endpoint structures

These belong in architecture docs or dev notes, not in specs.
