---
name: architecture-review
description: "Use when a PM needs to understand product risks hidden in system architecture — before planning features, during technical due diligence, or when evaluating feasibility of product changes."
---

# Architecture Review for Product Decisions

## Overview

Analyze system architecture from a product perspective. Surface risks, constraints, and opportunities that affect product planning — not just code quality. A Technical PM needs to know: what can we build fast, what's fragile, what blocks us, and what's a hidden superpower.

## When to Use

- Before writing a PRD for a feature touching complex systems
- Evaluating feasibility of a product idea
- Technical due diligence on a new project or acquisition
- Quarterly planning — understanding what's easy vs. hard to change
- When stakeholders ask "why can't we just..."

## Instructions

### Step 1: Map the System

Use codebase exploration to build a mental model:

1. **Identify entry points**: API routes, UI pages, CLI commands, event handlers
2. **Trace data flow**: How does data enter, transform, and persist?
3. **Map service boundaries**: What talks to what? Sync vs. async? Direct vs. queued?
4. **Catalog external dependencies**: Third-party APIs, databases, caches, message brokers

Use the Agent tool with code-explorer subagents to parallelize exploration across different system layers.

### Step 2: Assess Product Risk Dimensions

For each major component, evaluate:

| Dimension | Question | Impact on Product |
|-----------|----------|------------------|
| **Coupling** | How entangled are components? | High coupling = slow feature velocity, risky changes |
| **Data integrity** | Are there consistency guarantees? | Affects reliability promises to users |
| **Scalability ceiling** | Where does the system break under load? | Limits growth, pricing, and market expansion |
| **Extension points** | Where is the system designed to be extended? | Fast-path for new features |
| **Single points of failure** | What has no redundancy? | Uptime and SLA commitments |
| **Multi-tenancy** | How is tenant isolation achieved? | Affects enterprise readiness, security posture |

### Step 3: Identify the Product Implications

For each finding, translate to product language:

```
ARCHITECTURE FINDING: Order service directly calls inventory service synchronously
PRODUCT IMPLICATION: Adding a new fulfillment channel requires modifying the core order flow — high risk, 2-3 sprint effort minimum
RECOMMENDATION: If multi-channel fulfillment is on the roadmap, decouple first (1 sprint investment saves 3+ sprints per channel later)
```

### Step 4: Produce the Review Document

Structure the output as:

```markdown
# Architecture Review: [System/Feature Area]

## Executive Summary
[2-3 sentences: What's the system's current state? What's the biggest product risk?]

## System Map
[Component diagram or description of major boundaries]

## Strengths (Product Superpowers)
- [What's well-architected and enables fast product iteration?]

## Risks (Product Blockers)
| Risk | Severity | Product Impact | Mitigation |
|------|----------|---------------|------------|
| ... | High/Med/Low | ... | ... |

## Feature Feasibility Matrix
| Planned Feature | Feasibility | Effort Estimate | Key Constraint |
|----------------|-------------|-----------------|----------------|
| ... | Easy/Moderate/Hard/Requires Rearchitecture | ... | ... |

## Recommendations
1. [Prioritized list of architectural investments with product ROI]

## Raw Findings
[Detailed technical notes for engineering team]
```

### Step 5: Offer Follow-ups

- "Want me to deep-dive into any specific risk area?"
- "Should I draft a technical PRD for the highest-priority mitigation?"
- "Want me to estimate complexity for a specific planned feature?"

## Common Mistakes

- **Reviewing code quality instead of product impact** — This isn't a code review. Focus on what matters for product decisions.
- **Listing risks without severity or mitigation** — Every risk needs a "so what" and a "now what."
- **Ignoring what's working well** — Strengths inform where to double down, not just where to fix.
