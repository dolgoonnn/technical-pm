---
name: code-to-release-notes
description: "Use when generating release notes, changelogs, or stakeholder updates from git history — translating commits and diffs into user-facing language."
---

# Code to Release Notes

## Overview

Generate release notes by analyzing git history and code diffs. Translate technical changes into user-facing language that stakeholders, customers, and marketing teams can understand. The code is the source of truth — not someone's memory of what changed.

## When to Use

- Preparing a release for staging or production
- Writing a changelog for a sprint/milestone
- Stakeholder update: "what shipped this week?"
- Customer-facing release announcement
- App store / marketplace update description

## Instructions

### Step 1: Gather the Diff

Determine the range to analyze:

```bash
# Between two tags
git log v1.2.0..v1.3.0 --oneline --no-merges

# Between two dates
git log --after="2026-03-01" --before="2026-03-15" --oneline --no-merges

# Between branches
git log main..prod --oneline --no-merges

# Last N commits
git log -20 --oneline --no-merges
```

Read the full diff for significant commits to understand the actual changes, not just commit messages.

### Step 2: Categorize Changes

Group every change into one of these categories:

| Category | Includes | User Cares? |
|----------|----------|-------------|
| **New Features** | New user-facing capabilities | Yes — highlight |
| **Improvements** | Enhanced existing features, better UX | Yes — highlight |
| **Bug Fixes** | Resolved user-reported or internal bugs | Yes — brief |
| **Performance** | Speed, efficiency, resource usage | Sometimes |
| **Security** | Vulnerability patches, auth changes | Yes if user-facing |
| **Infrastructure** | DevOps, CI/CD, internal tooling | Rarely |
| **Refactoring** | Code restructuring, no behavior change | No — omit |
| **Dependencies** | Library updates | Only if security-related |

### Step 3: Translate to User Language

For each user-facing change, translate:

```
COMMIT: feat(orders): add bulk status update endpoint with optimistic locking
USER-FACING: You can now update the status of multiple orders at once, saving time when processing large batches.

COMMIT: fix(search): correct full-text index to handle CJK characters
USER-FACING: Fixed an issue where search wasn't finding products with Chinese, Japanese, or Korean characters in their names.

COMMIT: perf(dashboard): add Redis caching for analytics aggregation queries
USER-FACING: Dashboard loading times improved by up to 60% for accounts with large order volumes.
```

### Step 4: Produce Release Notes

Generate in the appropriate format based on audience:

**For stakeholders/customers:**
```markdown
# Release Notes — [Version/Date]

## What's New
- **[Feature Name]**: [1-2 sentence description of what users can now do]

## Improvements
- **[Area]**: [What got better and why it matters]

## Bug Fixes
- Fixed [user-visible symptom] ([ticket ID if applicable])

## Performance
- [Measurable improvement description]
```

**For engineering/internal:**
```markdown
# Changelog — [Version/Date]

## Added
- [feature description] ([PR #X])

## Changed
- [modification description] ([PR #X])

## Fixed
- [bug fix description] ([PR #X])

## Security
- [security patch description] ([PR #X])

## Infrastructure
- [internal change description] ([PR #X])
```

### Step 5: Offer Follow-ups

- "Want me to draft a customer email announcement from these notes?"
- "Should I generate a more detailed changelog for the engineering team?"
- "Want me to identify which changes need documentation updates?"

## Common Mistakes

- **Copy-pasting commit messages as release notes** — Commits are for engineers. Release notes are for humans.
- **Including internal refactoring** — Users don't care that you renamed a service. Only include behavior changes.
- **Missing the "so what"** — "Added bulk update endpoint" means nothing to a user. "You can now update 100 orders at once" does.
- **Forgetting to read the diff** — Commit messages lie. The diff is truth. Always verify what actually changed.
