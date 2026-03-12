---
name: debug-user-report
description: "Use when a user or customer reports a bug and you need to trace it through the codebase to find root cause, assess impact, and determine severity — before writing a ticket or escalating."
---

# Debug User Report

## Overview

Trace a user's bug report through the actual codebase to find the root cause, assess blast radius, and produce a properly-scoped bug ticket. Technical PMs who can triage with code-level understanding save engineering time and produce higher-quality bug reports.

## When to Use

- Customer support escalates a user-reported issue
- QA files a bug without clear reproduction steps
- Stakeholder reports something "broken" without technical details
- You need to assess severity before deciding priority
- You want to write a bug ticket that engineers can act on immediately

## Instructions

### Step 1: Parse the User Report

Extract from the report (ask user if missing):
- **What happened?** (observed behavior)
- **What was expected?** (correct behavior)
- **Steps to reproduce** (or at minimum: what page/feature, what action)
- **User context** (role, tenant, plan tier, browser/device if relevant)
- **Frequency** (always, sometimes, first time, since when)
- **Error messages** (exact text, screenshots, console output)

### Step 2: Trace Through the Codebase

Based on the symptom, work backward through the code:

**2a. Find the entry point**
- If UI issue: find the page/component (Grep for route paths, component names)
- If API issue: find the endpoint (Grep for URL paths, controller methods)
- If data issue: find the relevant models and their write paths

**2b. Trace the execution path**
- Follow the call chain from entry point to the point of failure
- Use Agent tool with code-explorer subagents to trace complex flows in parallel
- Note: check error handling at each step — many bugs live in catch blocks or missing validation

**2c. Identify the root cause**
- Look for: missing null checks, race conditions, incorrect query filters, stale cache, permission gaps, off-by-one errors, timezone issues, encoding problems
- Check recent git history for the affected files (`git log --oneline -10 -- <file>`) — was this a regression?

**2d. Assess blast radius**
- Who else is affected? (all users, specific tenant, specific plan, specific browser)
- What data is affected? (read-only display bug vs. data corruption)
- Are there related code paths with the same pattern? (Grep for similar patterns)

### Step 3: Determine Severity

| Severity | Criteria | Example |
|----------|----------|---------|
| **P0 - Critical** | Data loss, security breach, service down for all users | Payment processing silently fails |
| **P1 - High** | Core feature broken for a segment, workaround exists but painful | Search returns wrong results for multi-word queries |
| **P2 - Medium** | Feature degraded, easy workaround, affects some users | Export CSV missing one column |
| **P3 - Low** | Cosmetic, edge case, minimal user impact | Tooltip shows wrong timezone abbreviation |

### Step 4: Produce the Bug Report

```markdown
# BUG: [Short descriptive title]

## Severity: P[0-3]
## Reporter: [user/customer name or ticket ID]
## Affected: [who is impacted — all users? specific segment?]

## Summary
[1-2 sentences: what's broken and what's the user impact]

## Reproduction
1. [Step-by-step reproduction path]
2. ...
3. **Expected**: [correct behavior]
4. **Actual**: [broken behavior]

## Root Cause Analysis
**Location**: `[file:line]`
**Cause**: [technical explanation of why this happens]
**Introduced**: [commit/PR if regression, or "existing behavior" if always broken]

## Blast Radius
- **Users affected**: [scope]
- **Data impact**: [none / display only / data corruption]
- **Related patterns**: [other code with same bug pattern, if any]

## Suggested Fix
[Specific code change needed — not vague "fix the bug"]
- File: `[path]`
- Change: [what to modify]
- Test: [how to verify the fix]

## Workaround
[If any — for support team to share with affected users while fix is in progress]
```

### Step 5: Offer Follow-ups

- "Want me to check if this pattern exists elsewhere in the codebase?"
- "Should I estimate the fix complexity?"
- "Want me to draft a customer communication about this issue?"

## Common Mistakes

- **Reporting symptoms without root cause** — "Button doesn't work" is a support ticket, not an engineering ticket. Always trace to the code.
- **Guessing severity without blast radius analysis** — Check how many users/tenants are affected before assigning priority.
- **Missing the regression check** — Always check git history. If this used to work, finding the breaking commit is the fastest path to a fix.
