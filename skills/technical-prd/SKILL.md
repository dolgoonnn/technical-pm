---
name: technical-prd
description: "Use when writing a PRD that must include architecture constraints, API contracts, data model changes, or migration considerations — when the audience includes both engineers and product stakeholders."
---

# Technical PRD

## Overview

Write a Product Requirements Document grounded in the actual codebase. Unlike a standard PRD (user stories + acceptance criteria), a Technical PRD includes architecture constraints, API contracts, data model changes, and migration notes — because features don't exist in a vacuum.

## When to Use

- Feature requires schema changes or new data models
- Feature touches multiple services or bounded contexts
- Feature has non-obvious technical constraints that affect scope
- Engineering team needs more than user stories to estimate and plan
- Stakeholders need to understand WHY something takes longer than expected

## Instructions

### Step 1: Understand the Feature Request

Gather from the user:
- What problem does this solve? (user pain, business goal)
- Who are the users? (roles, personas, segments)
- What does success look like? (metrics, outcomes)
- What's the timeline pressure? (affects scope/approach tradeoffs)

If the user provides `$ARGUMENTS`, use that as the starting point.

### Step 2: Analyze the Codebase

Before writing a single requirement, understand the technical landscape:

1. **Find related code**: Use Grep/Glob to locate existing implementations in the feature area
2. **Trace the data model**: Read Prisma schemas, TypeORM entities, or equivalent — map all related models and their relationships (upward, downward, sideways)
3. **Map the API surface**: Find existing endpoints, GraphQL resolvers, or RPC methods in the area
4. **Identify existing patterns**: How does the codebase handle similar features? (CRUD patterns, event sourcing, saga patterns)
5. **Spot constraints**: Rate limits, tenant isolation, permission models, caching layers

Use the Agent tool with code-explorer subagents for parallel exploration.

### Step 3: Write the Technical PRD

```markdown
# [Feature Name] — Technical PRD

## Problem Statement
[What user/business problem does this solve? Include data if available.]

## Success Metrics
| Metric | Current | Target | Measurement |
|--------|---------|--------|-------------|
| ... | ... | ... | ... |

## User Stories
[Standard user stories with acceptance criteria]

---

## Technical Context

### Current Architecture
[How the relevant system works today — drawn from codebase analysis]

### Data Model Changes
[New models, modified fields, new relations — with Prisma/SQL/equivalent notation]

```prisma
// Example: New or modified models
model NewEntity {
  id        String   @id @default(cuid())
  // ... fields derived from codebase analysis
}
```

### API Changes
[New endpoints, modified contracts, deprecations]

| Method | Path | Request | Response | Notes |
|--------|------|---------|----------|-------|
| POST | /api/v1/... | `{ ... }` | `{ ... }` | New |
| PATCH | /api/v1/... | `{ ... }` | `{ ... }` | Modified |

### Migration Requirements
- [ ] Database migration: [describe schema changes]
- [ ] Data backfill: [if existing data needs transformation]
- [ ] Feature flag: [if gradual rollout needed]
- [ ] Backward compatibility: [API versioning considerations]

### Dependencies
| Dependency | Type | Risk | Notes |
|-----------|------|------|-------|
| [Service/Library] | Internal/External | Low/Med/High | ... |

### Architecture Decision Records
[Key decisions with rationale — why this approach over alternatives]

| Decision | Chosen Approach | Alternatives Considered | Rationale |
|----------|----------------|------------------------|-----------|
| ... | ... | ... | ... |

---

## Scope

### In Scope
- [Explicit list]

### Out of Scope
- [Explicit list with reasoning]

### Open Questions
- [Things that need resolution before/during implementation]

## Implementation Phases
[Break into shippable increments — each phase delivers user value]

### Phase 1: [Name] (Est: X sprints)
- [What's delivered]
- [Technical milestones]

### Phase 2: [Name] (Est: X sprints)
- [What's delivered]
- [Technical milestones]

## Rollback Plan
[How to safely revert if things go wrong]

## Security & Privacy
[Data handling, PII, access control, audit logging]
```

### Step 4: Validate Against Codebase

After drafting, verify:
- Do proposed data model changes conflict with existing relations?
- Do API changes follow existing conventions (naming, auth, pagination)?
- Are there existing utilities/services that can be reused?
- Does the migration path account for production data volumes?

### Step 5: Save and Offer Follow-ups

Save as `[feature-name]-technical-prd.md`.

Offer:
- "Want me to estimate complexity for each phase?"
- "Should I draft the migration plan in detail?"
- "Want me to create implementation tasks from this PRD?"

## Common Mistakes

- **Writing a standard PRD and calling it technical** — If there are no data models, API contracts, or migration notes, it's not a Technical PRD.
- **Including technical details without product context** — Engineers need to understand WHY, not just WHAT.
- **Proposing architecture without reading existing code** — Always ground proposals in what exists.
- **Forgetting rollback plans** — Every Technical PRD needs a "what if this goes wrong" section.
