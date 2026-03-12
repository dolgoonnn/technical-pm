---
description: Estimate feature effort from actual codebase analysis — not gut feel
argument-hint: "<feature description or user story>"
---

# /estimate-complexity -- Data-Driven Effort Estimation

Analyze the codebase to produce data-driven effort estimates. Counts affected files, traces dependency chains, and identifies risk multipliers.

## Invocation

```
/estimate-complexity add multi-language support to the product catalog
/estimate-complexity migrate from REST to GraphQL for the admin API
/estimate-complexity [paste user story or feature spec]
```

## Workflow

### Step 1: Define Scope

Ask the user:
- What exactly needs to be built? (specific behaviors)
- What's the definition of done? (shipped? tested? documented?)
- Any known constraints? (must use existing patterns, backward compatible)
- What team/developer will implement this? (experience with the area matters)

### Step 2: Analyze Codebase Impact

Apply the **estimate-complexity** skill:
- File impact analysis: find all files that need modification
- Dependency chain mapping: trace blast radius of changes
- Integration surface area: count boundaries crossed
- Pattern assessment: does a similar feature already exist?

### Step 3: Calculate Risk-Adjusted Estimate

- Apply risk multipliers (no existing pattern, auth changes, data migration, cross-service, no tests, legacy code, third-party APIs)
- Produce a range, not a single number
- State confidence level and key assumptions

### Step 4: Present the Estimate

```
## Complexity Estimate: [Feature]

| Metric | Value |
|--------|-------|
| Files affected | X |
| Services touched | X |
| Risk multiplier | Xx |
| Estimated effort | X-Y days |
| Confidence | High/Medium/Low |
```

Include per-component breakdown, dependency map, and risk assessment.

Offer:
- "Want me to break this into sprint-sized tasks?"
- "Should I write a Technical PRD for this feature?"
- "Want me to compare an alternative implementation approach?"

## Notes

- Always give a range (3-5 days), never a single point estimate (4 days).
- If confidence is Low, explain what would need to be resolved to increase it.
- Risk multipliers compound — a feature touching auth AND requiring migration AND crossing services is 1.3 x 1.5 x 1.5 = ~3x base effort.
