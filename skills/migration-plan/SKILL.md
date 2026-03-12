---
name: migration-plan
description: "Use when planning data migrations, infrastructure migrations, or major version upgrades — when you need a structured rollback strategy, risk assessment, and phased execution plan."
---

# Migration Plan

## Overview

Plan data, infrastructure, or platform migrations with structured risk assessment, phased execution, and rollback strategies. Migrations are the highest-risk operations in software — a Technical PM who can plan them properly prevents outages, data loss, and missed deadlines.

## When to Use

- Database schema migration on production data
- Moving from one service/provider to another (e.g., Heroku → AWS, REST → GraphQL)
- Major framework or library version upgrade
- Data format or encoding changes
- Multi-tenant data restructuring
- Merging or splitting databases/services

## Instructions

### Step 1: Assess Current State

Analyze the codebase to understand what's being migrated:

1. **Data inventory**: Read schema files, count records (estimate from code patterns), identify data relationships
2. **Consumer inventory**: Grep for all code that reads/writes the affected data — APIs, services, cron jobs, background workers
3. **External consumers**: Are there third-party integrations, webhooks, or public APIs that depend on the current structure?
4. **Data volume**: Estimate production data size (check seed files, model definitions, pagination defaults as proxies)

### Step 2: Design the Migration Strategy

Choose based on risk tolerance and data volume:

| Strategy | Best For | Risk | Downtime |
|----------|---------|------|----------|
| **Big bang** | Small data, low risk, short window | High | Yes |
| **Blue-green** | Stateless services, easy rollback needed | Medium | Minimal |
| **Strangler fig** | Large monolith decomposition | Low | None |
| **Dual-write** | Data migrations requiring zero downtime | Medium | None |
| **Feature flag** | Gradual rollout with instant rollback | Low | None |

### Step 3: Plan Phases

Break the migration into phases where each phase is independently deployable and rollback-safe:

**Phase 0: Preparation**
- Create migration scripts with dry-run capability
- Set up monitoring and alerting for migration metrics
- Document rollback procedures
- Communicate timeline to stakeholders

**Phase 1: Parallel Infrastructure**
- Deploy new structure alongside old
- Dual-write if applicable
- Verify data consistency between old and new

**Phase 2: Migration Execution**
- Run migration (with progress tracking)
- Validate data integrity at each checkpoint
- Monitor error rates, latency, and user-facing metrics

**Phase 3: Cutover**
- Switch reads to new structure
- Verify all consumers work correctly
- Keep old structure available for rollback window

**Phase 4: Cleanup**
- Remove dual-write code
- Drop old structures (after rollback window expires)
- Update documentation

### Step 4: Design the Rollback Strategy

For each phase, define:
- **Trigger**: What metrics/conditions trigger a rollback?
- **Procedure**: Exact steps to roll back (not "revert changes" — specific commands)
- **Data handling**: How to handle data written during the migration window
- **Communication**: Who gets notified and how

### Step 5: Produce the Migration Plan

```markdown
# Migration Plan: [Migration Name]

## Overview
| Aspect | Detail |
|--------|--------|
| What | [what's being migrated] |
| Why | [business/technical motivation] |
| Scope | [what's affected] |
| Strategy | [big bang / blue-green / strangler / dual-write / feature flag] |
| Timeline | [estimated phases and dates] |
| Rollback window | [how long we keep the ability to revert] |

## Risk Assessment
| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Data loss during migration | Low | Critical | Backup + dry-run validation |
| Extended downtime | Medium | High | Blue-green deployment, feature flag |
| Consumer breakage | Medium | High | Backward-compatible API, versioning |
| Performance degradation | Low | Medium | Load testing before cutover |

## Pre-Migration Checklist
- [ ] Full database backup verified and tested
- [ ] Migration script tested on staging with production-like data
- [ ] Rollback script tested on staging
- [ ] Monitoring dashboards configured
- [ ] Stakeholder communication sent
- [ ] On-call engineer identified for migration window
- [ ] Feature flags configured (if applicable)

## Phase Details

### Phase 0: Preparation [Date]
[Specific tasks with owners]

### Phase 1: Parallel Infrastructure [Date]
[Specific tasks with owners]

### Phase 2: Migration Execution [Date]
[Specific tasks with owners]
[Include specific validation queries/checks at each checkpoint]

### Phase 3: Cutover [Date]
[Specific tasks with owners]

### Phase 4: Cleanup [Date + rollback window]
[Specific tasks with owners]

## Rollback Plan
| Phase | Trigger | Procedure | Data Handling |
|-------|---------|-----------|---------------|
| Phase 1 | ... | ... | ... |
| Phase 2 | ... | ... | ... |
| Phase 3 | ... | ... | ... |

## Communication Plan
| Audience | When | Channel | Message |
|----------|------|---------|---------|
| Engineering | Pre-migration | Slack | Timeline + on-call details |
| Stakeholders | Pre-migration | Email | Timeline + expected impact |
| Users | If downtime | In-app + email | Maintenance window notice |
| Support | Pre-migration | Internal doc | FAQ for user questions |

## Success Criteria
- [ ] All data migrated with zero loss (validated by checksums)
- [ ] All consumers operating normally (monitored for 24h post-cutover)
- [ ] Performance within baseline thresholds
- [ ] No user-reported issues related to migration
```

### Step 6: Offer Follow-ups

- "Want me to write the migration scripts?"
- "Should I estimate the effort for each phase?"
- "Want me to draft the stakeholder communication?"

## Common Mistakes

- **No rollback plan** — Every migration must be reversible. "We'll figure it out" is not a rollback plan.
- **Testing only on empty databases** — Migration scripts that work on 100 rows often fail on 10 million. Test with realistic data volumes.
- **Forgetting background jobs** — Cron jobs and workers are consumers too. They break silently.
- **Single-phase "big bang" by default** — Phased migrations are almost always safer. Earn the right to big-bang by proving the risk is low.
