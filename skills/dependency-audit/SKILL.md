---
name: dependency-audit
description: "Use when assessing third-party dependency risks — before adopting a new library, during quarterly security reviews, or when evaluating vendor lock-in and supply chain risks."
---

# Dependency Audit

## Overview

Audit third-party dependencies for security vulnerabilities, licensing risks, maintenance health, and lock-in potential. Technical PMs need to understand dependency risk because it directly affects product reliability, security posture, and long-term maintainability.

## When to Use

- Evaluating whether to adopt a new dependency
- Quarterly or pre-release security/health review
- Incident response: was this caused by a dependency?
- Due diligence on a codebase (acquisition, migration)
- Stakeholder asks "are we secure?"

## Instructions

### Step 1: Inventory Dependencies

Scan the project's dependency manifests:

```bash
# Node.js
cat package.json | jq '.dependencies, .devDependencies'
npm audit

# Python
cat requirements.txt  # or Pipfile, pyproject.toml
pip audit

# Go
cat go.mod
go list -m all

# Rust
cat Cargo.toml
cargo audit
```

Read the actual manifest files. Count total dependencies (direct + transitive):
```bash
# Node.js transitive count
npm ls --all --json 2>/dev/null | jq '[.. | .version? // empty] | length'
```

### Step 2: Assess Each Critical Dependency

For direct dependencies (not devDependencies), evaluate:

| Dimension | How to Check | Risk Signal |
|-----------|-------------|-------------|
| **Security** | `npm audit` / `pip audit` / CVE databases | Known vulnerabilities, unpatched issues |
| **Maintenance** | GitHub: last commit, open issues, release cadence | No commits in 12+ months = abandoned |
| **License** | `package.json` license field, LICENSE file | GPL in commercial project = legal risk |
| **Popularity** | npm/PyPI download stats, GitHub stars | Very low adoption = higher abandonment risk |
| **Lock-in** | How deeply integrated? Easy to replace? | Deeply woven into business logic = high lock-in |
| **Bus factor** | How many maintainers? Corporate backing? | Single maintainer = supply chain risk |

Use web search to check recent CVEs and maintenance status for critical dependencies.

### Step 3: Identify High-Risk Patterns

Flag these patterns in the codebase:

- **Pinned to old major versions** (e.g., `lodash@3.x` when `4.x` is current)
- **Using deprecated packages** (check npm deprecation notices)
- **Forked dependencies** (patched locally instead of upstream)
- **Unnecessary dependencies** (using a library for something the language/framework provides natively)
- **Conflicting licenses** (mixing MIT, Apache, GPL in ways that create obligations)

### Step 4: Produce the Audit Report

```markdown
# Dependency Audit: [Project Name]

## Summary
| Metric | Value |
|--------|-------|
| Direct dependencies | X |
| Transitive dependencies | X |
| Known vulnerabilities | X critical, X high, X medium |
| Abandoned packages (12+ months) | X |
| License concerns | X |

## Critical Findings

### Security Vulnerabilities
| Package | Version | Severity | CVE | Fix Available? |
|---------|---------|----------|-----|----------------|
| ... | ... | Critical/High | ... | Yes/No |

**Action required**: [Immediate steps]

### Maintenance Risk
| Package | Last Release | Status | Impact if Abandoned |
|---------|-------------|--------|-------------------|
| ... | ... | Active/Stale/Abandoned | ... |

### License Concerns
| Package | License | Risk | Notes |
|---------|---------|------|-------|
| ... | ... | ... | ... |

## Dependency Health Scorecard
| Package | Security | Maintenance | License | Lock-in | Overall |
|---------|----------|-------------|---------|---------|---------|
| ... | G/Y/R | G/Y/R | G/Y/R | G/Y/R | G/Y/R |

## Recommendations
1. **Immediate** (security): [what to patch now]
2. **Short-term** (maintenance): [what to migrate away from]
3. **Long-term** (architecture): [reduce lock-in on X]

## Alternatives Assessment
[For high-risk dependencies, list viable alternatives]

| Current | Alternative | Migration Effort | Trade-offs |
|---------|-------------|-----------------|------------|
| ... | ... | ... | ... |
```

### Step 5: Offer Follow-ups

- "Want me to create tickets for the critical security fixes?"
- "Should I estimate the effort to migrate away from [risky dependency]?"
- "Want me to set up automated dependency scanning in CI?"

## Common Mistakes

- **Only checking direct dependencies** — Transitive dependencies are where most vulnerabilities hide.
- **Ignoring license compliance** — GPL in a commercial SaaS product is a legal problem, not just a technical one.
- **Treating all dependencies equally** — Focus audit depth on dependencies in the critical path (auth, payments, data handling).
