---
description: Trace a user bug report through the codebase to find root cause and assess impact
argument-hint: "<bug report, user complaint, or support ticket>"
---

# /debug-user-report -- Code-Level Bug Triage

Trace a user's bug report through the actual codebase to find root cause, assess blast radius, and produce an actionable engineering ticket.

## Invocation

```
/debug-user-report "Customer says orders page shows wrong total after applying discount code"
/debug-user-report "User can't upload images larger than 2MB even though limit should be 10MB"
/debug-user-report [paste support ticket or user report]
```

## Workflow

### Step 1: Parse the Report

Extract or ask for:
- What happened vs. what was expected?
- Steps to reproduce (or at minimum: feature area + action)
- User context (role, tenant, plan tier)
- Frequency and error messages

### Step 2: Trace Through Code

Apply the **debug-user-report** skill:
- Find the entry point (UI component, API endpoint, data path)
- Trace execution path from entry to failure point
- Identify root cause with file:line reference
- Check git history for regression
- Assess blast radius (who else is affected, data impact)

### Step 3: Determine Severity

- P0 (Critical): data loss, security, service down
- P1 (High): core feature broken, painful workaround
- P2 (Medium): feature degraded, easy workaround
- P3 (Low): cosmetic, edge case

### Step 4: Produce Bug Report

```
# BUG: [Title]
## Severity: P[0-3]
## Summary: [what's broken and user impact]
## Root Cause: [file:line] — [technical explanation]
## Blast Radius: [who's affected, data impact]
## Suggested Fix: [specific code change]
## Workaround: [for support team]
```

Offer:
- "Want me to check if this pattern exists elsewhere?"
- "Should I estimate the fix complexity?"
- "Want me to draft a customer communication?"

## Notes

- Always check git history for the affected files — regressions are the fastest to fix and the easiest to explain.
- Don't stop at symptoms. "Button doesn't work" needs to become "onClick handler doesn't fire because the event listener is attached to a stale ref."
