---
description: Generate release notes from git history — translate commits into user-facing language
argument-hint: "<version range, date range, or branch comparison>"
---

# /release-notes -- Code to Release Notes

Generate release notes by analyzing git history and code diffs. Translate technical changes into language that stakeholders and customers understand.

## Invocation

```
/release-notes v1.2.0..v1.3.0
/release-notes last 2 weeks
/release-notes main..prod
/release-notes since last release
```

## Workflow

### Step 1: Determine Range

Ask the user (if not provided):
- What range? (tag-to-tag, date range, branch comparison, last N commits)
- What audience? (customers, stakeholders, engineering team, app store)
- What format? (changelog, announcement, email, in-app notification)

### Step 2: Analyze Changes

Apply the **code-to-release-notes** skill:
- Read git log for the specified range
- Read diffs for significant commits to understand actual changes
- Categorize: New Features, Improvements, Bug Fixes, Performance, Security, Infrastructure
- Filter out internal-only changes (refactoring, CI, dev tooling)

### Step 3: Translate to User Language

For each user-facing change:
- Convert technical description to user benefit
- Include specific improvements ("60% faster" not "improved performance")
- Group related changes under clear headings

### Step 4: Present Release Notes

**Customer-facing format:**
```
# What's New — [Version/Date]

## New Features
- **[Feature]**: [what you can now do]

## Improvements
- **[Area]**: [what got better]

## Bug Fixes
- Fixed [user-visible symptom]
```

**Engineering format:**
```
# Changelog — [Version/Date]

## Added / Changed / Fixed / Security / Infrastructure
- [description] ([PR #X])
```

Offer:
- "Want me to draft a customer email announcement?"
- "Should I generate a detailed engineering changelog?"
- "Want me to identify changes that need documentation updates?"

## Notes

- Commit messages are for engineers. Release notes are for humans. Always translate.
- Omit refactoring, internal tooling, and CI changes from customer-facing notes.
- Read the actual diff — commit messages often don't tell the full story.
