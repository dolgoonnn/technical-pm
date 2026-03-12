---
description: Plan a data or infrastructure migration with rollback strategy and risk assessment
argument-hint: "<what's being migrated and why>"
---

# /plan-migration -- Structured Migration Planning

Plan data, infrastructure, or platform migrations with phased execution, risk assessment, and rollback strategies.

## Invocation

```
/plan-migration PostgreSQL schema migration — adding multi-currency to orders
/plan-migration move from Heroku to AWS ECS
/plan-migration upgrade Next.js from 13 to 15
/plan-migration merge user and account tables
```

## Workflow

### Step 1: Understand the Migration

Ask the user:
- What's being migrated? (schema, infrastructure, framework, data format)
- Why? (business requirement, technical debt, compliance, performance)
- What's the timeline? (urgent vs. planned)
- What's the risk tolerance? (zero downtime required? maintenance window ok?)
- Who are the stakeholders? (engineering, product, customers)

### Step 2: Analyze Current State

Apply the **migration-plan** skill:
- Read schemas, configs, and code to understand current state
- Inventory all consumers of the affected data/systems
- Estimate data volumes and complexity
- Identify external integrations and dependencies

### Step 3: Design Strategy and Phases

Choose migration strategy (big bang, blue-green, strangler fig, dual-write, feature flag) based on risk tolerance and data volume.

Break into independently-deployable phases:
- Phase 0: Preparation (scripts, monitoring, communication)
- Phase 1: Parallel infrastructure
- Phase 2: Migration execution
- Phase 3: Cutover
- Phase 4: Cleanup

Design rollback triggers, procedures, and data handling for each phase.

### Step 4: Deliver the Plan

Present the complete migration plan with:
- Overview (what, why, scope, strategy, timeline, rollback window)
- Risk assessment with mitigations
- Pre-migration checklist
- Phase details with specific tasks and owners
- Rollback plan per phase
- Communication plan by audience
- Success criteria

Save as `migration-plan-[name]-[date].md`.

Offer:
- "Want me to write the migration scripts?"
- "Should I estimate effort for each phase?"
- "Want me to draft the stakeholder communication?"

## Notes

- Every phase must be independently rollback-safe. If you can't roll back a phase, it's too big — break it down further.
- Always test migration scripts with production-like data volumes, not empty databases.
- Don't forget background jobs, cron tasks, and async workers — they're consumers too.
