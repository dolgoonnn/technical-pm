---
name: incident-postmortem
description: "Use after an incident, outage, or production issue to produce a structured blameless postmortem — with timeline, root cause analysis, blast radius, and prevention actions traced through the actual code."
---

# Incident Postmortem

## Overview

Produce a structured, blameless postmortem by analyzing the actual code, git history, and system architecture. Go beyond "what happened" to understand WHY it happened at the code level and HOW to prevent it structurally — not just "be more careful next time."

## When to Use

- After a production incident (P0-P2)
- After a near-miss that could have been worse
- Recurring issue that keeps coming back
- Stakeholders need a formal incident report
- Team wants to learn from a failure

## Instructions

### Step 1: Establish the Timeline

Reconstruct the incident chronologically:

1. **Check git history** around the incident window:
   ```bash
   git log --after="YYYY-MM-DD" --before="YYYY-MM-DD" --oneline --all
   ```
2. **Identify the triggering change** (if deployment-related):
   ```bash
   git diff <before-commit>..<after-commit> --stat
   ```
3. **Review the affected code paths** — read the files that were changed or involved

Build the timeline:
- **Trigger**: What initiated the incident? (deploy, config change, traffic spike, external dependency)
- **Detection**: When was it noticed? How? (monitoring, user report, internal discovery)
- **Response**: What actions were taken? In what order?
- **Resolution**: What fixed it? When was it confirmed resolved?

### Step 2: Root Cause Analysis

Trace through the code to find the actual root cause:

**2a. Proximate cause** — What directly broke?
- Read the relevant code files
- Check the git diff of the triggering change
- Identify the exact line(s) or condition that failed

**2b. Contributing factors** — Why wasn't this caught?
- Was there test coverage? (Check test files for the affected code)
- Was there monitoring/alerting? (Check for health checks, metrics, alerts)
- Was there a review? (Check PR/commit for review comments)
- Did the staging environment reproduce this? (Check environment differences)

**2c. Systemic cause** — Why does the system allow this class of failure?
- Is this a pattern that could recur? (Grep for similar patterns)
- Are there other code paths with the same vulnerability?
- What architectural weakness enabled this?

Use the **5 Whys** technique:
```
Why did the API return 500? → Null pointer in user lookup
Why was user null? → Cache returned stale entry after deletion
Why wasn't cache invalidated? → Delete endpoint doesn't call cache.invalidate()
Why not? → No cache invalidation pattern enforced in codebase
Why not? → No middleware/decorator pattern for cache consistency
ROOT CAUSE: Missing architectural pattern for cache invalidation
```

### Step 3: Assess Impact

| Dimension | Assessment |
|-----------|-----------|
| **Duration** | Time from trigger to resolution |
| **Users affected** | Count or percentage |
| **Data impact** | Was data lost, corrupted, or exposed? |
| **Revenue impact** | Transactions failed, SLA breach, refunds |
| **Reputation impact** | Public visibility, customer trust |
| **Severity** | P0/P1/P2 with justification |

### Step 4: Define Action Items

Every action item must be:
- **Specific** (not "improve monitoring" but "add latency P99 alert for /api/orders endpoint")
- **Owned** (assigned to a person or team)
- **Timebound** (due date or sprint)
- **Categorized** by type:

| Type | Purpose | Example |
|------|---------|---------|
| **Detect** | Catch this faster next time | Add alert for error rate > 5% on payment endpoints |
| **Prevent** | Stop this class of bug | Add cache invalidation middleware to all write endpoints |
| **Mitigate** | Reduce impact if it recurs | Add circuit breaker to external payment API calls |
| **Process** | Improve human response | Update runbook with cache flush procedure |

### Step 5: Produce the Postmortem

```markdown
# Incident Postmortem: [Incident Title]

**Date**: [incident date]
**Severity**: P[0-3]
**Duration**: [start → resolution]
**Author**: [who wrote this]
**Status**: [draft / reviewed / final]

## Executive Summary
[2-3 sentences: what happened, who was affected, what we're doing about it]

## Timeline
| Time (UTC) | Event |
|-----------|-------|
| HH:MM | [Triggering event — deploy, config change, traffic spike] |
| HH:MM | [First symptom detected] |
| HH:MM | [Response action taken] |
| HH:MM | [Resolution deployed] |
| HH:MM | [Confirmed resolved] |

## Root Cause
**Proximate**: [what directly broke — with file:line reference]
**Contributing**: [what allowed it to happen — missing tests, monitoring gaps]
**Systemic**: [what architectural weakness enabled this class of failure]

### 5 Whys
1. Why? → [answer]
2. Why? → [answer]
3. Why? → [answer]
4. Why? → [answer]
5. Why? → [ROOT CAUSE]

## Impact
| Dimension | Assessment |
|-----------|-----------|
| Users affected | ... |
| Duration | ... |
| Data impact | ... |
| Revenue impact | ... |

## What Went Well
- [Things that worked during response]

## What Went Wrong
- [Things that made the incident worse or delayed resolution]

## Action Items
| # | Action | Type | Owner | Due | Status |
|---|--------|------|-------|-----|--------|
| 1 | ... | Detect/Prevent/Mitigate/Process | ... | ... | Open |
| 2 | ... | ... | ... | ... | Open |

## Lessons Learned
[Key takeaways for the team — focus on systemic improvements, not individual blame]

## Related Incidents
[Links to similar past incidents, if any — pattern detection]
```

### Step 6: Offer Follow-ups

- "Want me to check if the same vulnerability exists elsewhere in the codebase?"
- "Should I draft the action items as engineering tickets?"
- "Want me to write the customer communication about this incident?"

## Common Mistakes

- **Blame individuals instead of systems** — "Bob deployed bad code" is not a root cause. "No automated test for this edge case" is.
- **Shallow root cause** — If your root cause is "a bug in the code," keep asking why. What allowed that bug to exist?
- **Vague action items** — "Improve monitoring" is not actionable. "Add P99 latency alert on /api/payments with 500ms threshold" is.
- **No systemic analysis** — If you only fix the immediate bug, the same class of failure will recur. Find the architectural pattern.
