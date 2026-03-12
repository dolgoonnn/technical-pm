---
description: Audit third-party dependency risks — security, licensing, maintenance health, and lock-in
argument-hint: "<'full audit', specific package name, or concern area>"
---

# /audit-dependencies -- Dependency Health Check

Assess third-party dependency risks: security vulnerabilities, licensing concerns, maintenance health, and vendor lock-in.

## Invocation

```
/audit-dependencies full audit
/audit-dependencies stripe — considering switching payment providers
/audit-dependencies security focus — preparing for SOC2 audit
/audit-dependencies [specific package name]
```

## Workflow

### Step 1: Scope the Audit

Ask the user (if not clear from arguments):
- Full audit or focused on specific packages/concerns?
- What's the context? (quarterly review, pre-release, evaluating new dependency, compliance)
- Any specific risk areas? (security, licensing, maintenance, performance)

### Step 2: Inventory and Analyze

Apply the **dependency-audit** skill:
- Read dependency manifests (package.json, requirements.txt, go.mod, etc.)
- Run security audit tools (npm audit, pip audit, etc.)
- Assess each critical dependency across: security, maintenance, license, popularity, lock-in, bus factor
- Identify high-risk patterns (old versions, deprecated packages, unnecessary deps)

### Step 3: Deliver the Report

Present the structured audit:
- Summary metrics (total deps, vulnerabilities, abandoned packages)
- Critical findings (security, maintenance, license)
- Health scorecard (Green/Yellow/Red per dimension)
- Prioritized recommendations (immediate, short-term, long-term)
- Alternative assessment for high-risk dependencies

Save as `dependency-audit-[date].md`.

Offer:
- "Want me to create tickets for the critical security fixes?"
- "Should I estimate migration effort for [risky dependency]?"
- "Want me to set up automated scanning recommendations?"

## Notes

- Focus depth on dependencies in the critical path (auth, payments, data handling) — not dev tooling.
- Check transitive dependencies, not just direct ones — that's where vulnerabilities hide.
- License compliance is a legal issue, not just technical. Flag GPL in commercial projects.
