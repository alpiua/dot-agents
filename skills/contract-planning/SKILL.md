---
name: contract-planning
description: Build or amend an implementation task contract from its dependency graph, live owners, future consumers, failure modes, and measurable proof.
---

# Contract planning

Use before creating, reopening, materially amending, or admitting an
implementation task. Use it again when review or repair shows that the current
contract omitted an owner, consumer, failure mode, or proof obligation.

## Load the repository context

1. Read the repository's `AGENTS.md` and this skill's `ADDON.md`, when one
   exists. The add-on supplies repository-specific planning authority, tools,
   commands, and the local lesson location; it may strengthen this procedure,
   never weaken it.
2. Read all current local lessons named by the add-on before drafting or
   amending the contract. Validate them against the current authority.

## Discover the whole contract boundary

1. Read the active task, its parent plan or phase, its milestone, and its
   explicit prerequisites and successors.
2. Search the bounded planning scope for the proposed capability's task ID,
   type or interface, configuration key, runtime caller, test name, and named
   consumer. Read every matching producer and consumer before choosing an
   owner.
3. Verify present callers, contracts, configuration resolution, storage, and
   test seams in the live implementation. Record an unavailable architecture
   index or currentness response as a coverage gap.
4. Make a producer-to-consumer map that names current callers and planned
   consumers separately. A planned consumer defines a compatibility or deferral
   boundary; it does not authorize speculative implementation.

## Freeze an executable architecture

For each affected surface, record its owner, input and output, authority,
durable state, configuration/default, current and planned consumer, fail-closed
behavior, proof, and deferred owner. Resolve each as change, prove unchanged,
linked defer, or justified N/A.

Trace at least one complete path:

```text
trigger -> production caller -> typed contract -> canonical owner -> storage or runtime propagation -> consumer -> observable result or rejection
```

For every path decide authorization, tenancy or equivalent scope, validation,
bounds, retry and cleanup behavior, alternate backend or transport behavior,
rollback or compensation, and forbidden effects. Put unresolved architectural
choices in `AUTHORITY_GAP`; do not hide them in Build or review repairs.

## Specify proof before Build

Name the exact test seam and sensitivity mutation for every accepted claim. Keep
hermetic and live lanes distinct: each declares its configuration source,
required infrastructure, data shape, and cleanup owner.

Every latency, throughput, size, or count promise names the production
operation, warmup policy, fixture/data cardinality, measured interval, sample
count, percentile method, target, environment, and deliberate regression
mutation. A helper microbenchmark cannot replace a caller-path measurement when
the task claims caller-path performance.

## Learn at Handoff

Review the session's Contract changes, dispositioned review findings, and failed
or misleading checks. Derive zero or more candidates from verified evidence.
Admit each candidate only when it prevents a contract or handoff failure,
applies beyond the session, and is independent of every admitted candidate.

Before adding a rule, compare its prevented failure and required action with all
current local lessons: skip an equivalent or subsumed candidate, and amend an
existing rule only when evidence supports a strictly clearer or broader rule.
When no candidate qualifies, do not edit the lesson record. Preserve supporting
evidence in the planning or review authority; do not copy transcripts, raw tool
output, routine progress, or an unverified diagnosis. A lesson is evidence; it
never creates authority or amends the current Contract.
