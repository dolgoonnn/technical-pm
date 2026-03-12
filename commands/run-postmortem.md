---
description: Run a structured blameless incident postmortem with code-level root cause analysis
argument-hint: "<incident description, date, or 'recent incident'>"
---

# /run-postmortem -- Incident Postmortem

Produce a structured, blameless postmortem by analyzing code, git history, and system architecture. Find root causes at the code level, not just "human error."

## Invocation

```
/run-postmortem payment processing was down for 2 hours yesterday
/run-postmortem users couldn't login between 2pm-4pm UTC on March 10
/run-postmortem [paste incident report or alert details]
```

## Workflow

### Step 1: Establish Context

Ask the user:
- What happened? (symptoms, services affected)
- When? (start time, detection time, resolution time)
- Was there a triggering event? (deploy, config change, traffic spike)
- What's the impact? (users affected, data impact, revenue impact)
- Who responded? (for timeline accuracy)

### Step 2: Investigate Through Code

Apply the **incident-postmortem** skill:
- Check git history around the incident window for triggering changes
- Read the affected code paths
- Trace root cause: proximate → contributing → systemic
- Apply 5 Whys technique to reach architectural root cause
- Assess blast radius from code analysis

### Step 3: Produce the Postmortem

Present the structured document:
- Executive Summary
- Timeline (chronological events)
- Root Cause (proximate, contributing, systemic + 5 Whys)
- Impact assessment
- What went well / What went wrong
- Action items (Detect, Prevent, Mitigate, Process) — specific, owned, timebound
- Lessons learned
- Related incidents

Save as `postmortem-[incident]-[date].md`.

Offer:
- "Want me to check if the same vulnerability exists elsewhere in the codebase?"
- "Should I draft the action items as engineering tickets?"
- "Want me to write the customer communication?"
- "Should I estimate effort for the prevention action items?"

## Notes

- Blameless means blameless. "Bob deployed bad code" is never a root cause. "No automated test for edge case X" is.
- If the root cause is "human error," keep asking why. What system allowed that error to happen?
- Every action item must be specific enough to be a ticket. "Improve monitoring" is not actionable.
