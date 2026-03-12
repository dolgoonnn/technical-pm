# technical-pm

**Code-aware PM skills for Claude Code.** Bridge the gap between engineering and product — skills that read your codebase and produce technically-grounded product artifacts.

## What Makes This Different

Regular PM skills operate on frameworks and templates. Technical PM skills **read your actual codebase** — tracing data flows, analyzing dependencies, mapping architecture — to produce artifacts grounded in engineering reality.

| Regular PM Skill | Technical PM Skill |
|-----------------|-------------------|
| "Write a PRD" | "Write a PRD informed by the actual codebase architecture" |
| "Estimate effort" | "Analyze dependency graph and complexity to estimate effort" |
| "Release notes" | "Release notes auto-generated from git diff + commit messages" |
| "Debug user report" | "Trace user bug report through code to find root cause" |
| "Assess risk" | "Audit actual dependencies for security, licensing, maintenance risk" |

## Skills

| Skill | What It Does |
|-------|-------------|
| `architecture-review` | Review system design from a PM perspective — surface product risks hidden in architecture |
| `technical-prd` | Write PRDs with architecture constraints, API specs, data model changes, and migration notes |
| `estimate-complexity` | Analyze the codebase dependency graph to produce data-driven effort estimates |
| `debug-user-report` | Trace a user's bug report through the codebase to find root cause and impact |
| `code-to-release-notes` | Generate release notes from git diff and commit history |
| `dependency-audit` | Assess third-party dependency risks: security, licensing, maintenance, alternatives |
| `migration-plan` | Plan data/infrastructure migrations with rollback strategies and risk assessment |
| `incident-postmortem` | Structured incident analysis: timeline, root cause, blast radius, prevention |

## Commands

| Command | Usage |
|---------|-------|
| `/technical-pm:review-architecture` | Review architecture for product risks |
| `/technical-pm:write-technical-prd` | Write a code-aware PRD |
| `/technical-pm:estimate-complexity` | Estimate feature effort from codebase analysis |
| `/technical-pm:debug-user-report` | Trace a user bug report through code |
| `/technical-pm:release-notes` | Generate release notes from git history |
| `/technical-pm:audit-dependencies` | Audit dependency health and risks |
| `/technical-pm:plan-migration` | Plan a migration with rollback strategy |
| `/technical-pm:run-postmortem` | Run a structured incident postmortem |

## Installation

```bash
claude plugin install technical-pm
```

Or manually add to `~/.claude/plugins/installed_plugins.json`.

## Who This Is For

**Technical PMs** who sit between engineering and product — people who can read code, understand architecture, AND drive product decisions. These skills combine engineering analysis with PM frameworks to produce artifacts that both engineers and stakeholders trust.

## Complementary Plugins

- **[pm-skills](https://github.com/phuryn/pm-skills)** — Framework-heavy PM skills (Lean Canvas, SWOT, Porter's Five Forces). Great for pure product work.
- **[superpowers](https://github.com/obra/superpowers)** — Engineering workflow skills (TDD, debugging, planning). Great for pure engineering work.
- **technical-pm** — The bridge. Code-aware PM skills for people who live in both worlds.

## License

MIT
