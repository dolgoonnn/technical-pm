---
description: Review system architecture from a PM perspective — surface product risks hidden in code
argument-hint: "<system area, feature, or 'full system'>"
---

# /review-architecture -- Architecture Review for Product Decisions

Analyze system architecture from a product perspective. Surfaces risks, constraints, and opportunities that affect product planning.

## Invocation

```
/review-architecture order processing system
/review-architecture full system — we're planning Q3 roadmap
/review-architecture payment integration — evaluating switching providers
```

## Workflow

### Step 1: Scope the Review

Ask the user:
- What system area or feature should I review? (or "full system" for broad review)
- What's the context? (roadmap planning, feasibility check, due diligence, incident follow-up)
- Any specific concerns? (scalability, reliability, vendor lock-in, developer velocity)

### Step 2: Explore the Codebase

Apply the **architecture-review** skill:
- Launch code-explorer subagents to map system boundaries, data flows, and integration points
- Assess coupling, scalability, extension points, single points of failure
- Translate every technical finding to product impact

### Step 3: Deliver the Review

Present findings in the structured format:
- Executive Summary (2-3 sentences)
- System Map
- Strengths (Product Superpowers)
- Risks (Product Blockers) with severity and mitigation
- Feature Feasibility Matrix
- Prioritized Recommendations

Save as `architecture-review-[area]-[date].md`.

Offer:
- "Want me to deep-dive into any specific risk area?"
- "Should I draft a technical PRD for the highest-priority mitigation?"
- "Want me to estimate complexity for a specific planned feature?"

## Notes

- This is NOT a code review. Focus on product impact, not code quality.
- Every risk must have a "so what" (product impact) and a "now what" (mitigation).
- Include strengths — knowing what works well is as important as knowing what's broken.
