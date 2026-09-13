---
name: acdd-flow
description: Route planned delivery work to contract planning, delivery orchestration, review authority, and handoff.
---

# ACDD Flow

Use this skill as the entry point for planned delivery work. It owns no host,
repository policy, contract schema, agent topology, or evidence store.

## Route

1. Read the task or plan and the owning repository's `AGENTS.md`.
2. Load `contract-planning` before drafting, rescanning, reopening, or changing
   a Contract. Read its repository `ADDON.md` when present.
3. Load `delivery-orchestration` to schedule admitted Build, Test, and review
   work. It does not change the frozen Contract.
4. Load `review-authority` before classifying a finding or admitting a repair.
   It routes findings outside current authority to a decision or deferred work.
5. Load `handoff` after closure evidence is settled. Read its repository
   `ADDON.md` when present to publish the result, deferred work, and durable
   evidence to the owning planner or task record.

Repository-specific commands, paths, models, permissions, runtime mechanics,
and artifact locations belong to the receiving skill's add-on or repository
instructions. Do not create an `acdd-flow` add-on.
