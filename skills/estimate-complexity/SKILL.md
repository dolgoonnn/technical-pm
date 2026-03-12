---
name: estimate-complexity
description: "Use when estimating feature effort based on actual codebase analysis — not gut feel. When planning sprints, scoping work, or answering 'how long will this take' with data."
---

# Estimate Feature Complexity

## Overview

Produce data-driven effort estimates by analyzing the actual codebase — counting affected files, tracing dependency chains, measuring integration surface area, and identifying risk multipliers. Replaces "t-shirt sizing by vibes" with structured analysis.

## When to Use

- Sprint planning: sizing stories with engineering input
- Stakeholder asks "how long will X take?"
- Comparing approaches: which path has lower implementation cost?
- Prioritization: effort vs. impact scoring
- Feasibility check before committing to a deadline

## Instructions

### Step 1: Define the Feature Scope

Clarify with the user:
- What exactly needs to be built? (specific behaviors, not vague goals)
- What's the definition of done? (shipped? tested? documented?)
- Any known constraints? (must use existing patterns, backward compatible, etc.)

### Step 2: Analyze Codebase Impact

For each component the feature touches, analyze:

**2a. File Impact Analysis**
- Use Grep/Glob to find all files that will need modification
- Count files by category: models, services, controllers, tests, migrations, UI components
- Flag files with high change frequency (use `git log --format='%H' -- <file> | wc -l` as a proxy for complexity/risk)

**2b. Dependency Chain Mapping**
- Trace what the changed code depends on (imports, service calls, shared state)
- Trace what depends on the changed code (consumers, downstream effects)
- Identify "blast radius" — how far do changes propagate?

**2c. Integration Surface Area**
- Count external API integrations affected
- Count internal service boundaries crossed
- Identify shared data stores or caches that need coordination

**2d. Existing Pattern Assessment**
- Does a similar feature already exist? (copy-modify is faster than greenfield)
- Are the right abstractions in place? (or does new infrastructure need to be built?)
- Is there test infrastructure for this area? (or does testing scaffolding need to be created?)

### Step 3: Identify Risk Multipliers

| Risk Factor | Multiplier | Detection |
|-------------|-----------|-----------|
| No existing pattern to follow | 1.5x | Grep for similar implementations returns nothing |
| Touches auth/permissions | 1.3x | Files in auth/, middleware/, guards/ affected |
| Requires data migration | 1.5-2x | Schema changes on tables with production data |
| Cross-service coordination | 1.5x | Changes span multiple service boundaries |
| No test coverage in area | 1.3x | Test files missing or minimal for affected code |
| Unfamiliar/legacy code | 1.5x | Code lacks types, comments, or clear patterns |
| Third-party API dependency | 1.3x | External API calls in the critical path |

### Step 4: Produce the Estimate

```markdown
# Complexity Estimate: [Feature Name]

## Summary
| Metric | Value |
|--------|-------|
| Files affected | X |
| Services/modules touched | X |
| New files needed | X |
| Risk multiplier | Xx |
| Estimated effort | X-Y days (X-Y story points) |
| Confidence | High/Medium/Low |

## Breakdown by Component

### [Component 1]
- **Files**: [list with paths]
- **Changes**: [what needs to change]
- **Base effort**: X days
- **Risk factors**: [applicable multipliers]
- **Adjusted effort**: X days

### [Component 2]
[same structure]

## Dependency Map
[Which components must be done in sequence vs. can be parallelized]

```
[Component A] → [Component B] → [Component C]
                                       ↑
[Component D] ─────────────────────────┘
```

## Risk Assessment
| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| ... | High/Med/Low | ... | ... |

## Confidence Notes
- [What could make this estimate wrong?]
- [What assumptions are baked in?]
- [What unknowns remain?]

## Comparison (if multiple approaches)
| Approach | Effort | Risk | Maintainability |
|----------|--------|------|-----------------|
| A: ... | X days | Low | High |
| B: ... | Y days | Med | Med |
```

### Step 5: Offer Follow-ups

- "Want me to break this into sprint-sized tasks?"
- "Should I write a Technical PRD for this feature?"
- "Want me to compare an alternative implementation approach?"

## Common Mistakes

- **Estimating without reading the code** — The whole point is data-driven estimation. If you didn't Grep, you're guessing.
- **Forgetting risk multipliers** — Base effort x risk factors = realistic estimate. Base effort alone is optimistic fiction.
- **Single-point estimates** — Always give a range. "3-5 days" is more honest than "4 days."
- **Ignoring test effort** — If the area has no tests, testing infrastructure is part of the cost.
