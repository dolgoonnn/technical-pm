---
description: Write a PRD grounded in actual codebase architecture — includes API contracts, data models, and migration notes
argument-hint: "<feature description or problem statement>"
---

# /write-technical-prd -- Code-Aware Product Requirements Document

Write a PRD that includes architecture constraints, API contracts, data model changes, and migration considerations — because features don't exist in a vacuum.

## Invocation

```
/write-technical-prd bulk order editing for admin dashboard
/write-technical-prd multi-currency support for the payment system
/write-technical-prd [paste feature request or user story]
```

## Workflow

### Step 1: Understand the Feature

Ask the user:
- What problem does this solve? (user pain, business goal)
- Who are the users? (roles, personas, segments)
- What does success look like? (metrics, outcomes)
- What's the timeline pressure? (affects scope/approach tradeoffs)
- Any existing specs or designs to incorporate?

### Step 2: Analyze the Codebase

Apply the **technical-prd** skill:
- Explore related code: data models, API endpoints, existing patterns
- Trace entity relationships (upward, downward, sideways)
- Identify constraints: permissions, tenant isolation, caching, rate limits
- Find reusable patterns and infrastructure

### Step 3: Write the Technical PRD

Produce the full document:
- Problem Statement + Success Metrics
- User Stories with acceptance criteria
- Technical Context (current architecture)
- Data Model Changes (with schema notation)
- API Changes (method, path, request/response)
- Migration Requirements
- Dependencies and ADRs
- Scope (in/out) + Open Questions
- Implementation Phases with estimates
- Rollback Plan + Security/Privacy considerations

Save as `[feature-name]-technical-prd.md`.

Offer:
- "Want me to estimate complexity for each phase?"
- "Should I draft the migration plan in detail?"
- "Want me to create implementation tasks from this PRD?"
- "Should I review this against the existing architecture for conflicts?"

## Notes

- Always ground proposals in existing codebase patterns — don't propose architecture that contradicts what exists without explicitly calling it out.
- If the feature requires schema changes, always include migration strategy and rollback plan.
- If you can't determine something from the code, flag it as an open question — don't guess.
