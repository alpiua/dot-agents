---
name: delivery-orchestration
description: Orchestrate bounded build, test, and review agents with consolidated findings, stable evidence, and proof gates.
---

# Delivery Orchestration

Use this skill to coordinate implementation and review. It owns session topology
and review cadence, not review authority, defect semantics, or repository policy.

## Required Skills

1. Load `review-authority` before launching reviewers.
2. Load `code-quality` for non-trivial code, schema, configuration, or test work.
3. Select an independent reviewer under repository policy; do not assume a
   host-specific agent name.
4. Load `close-review-findings` for every concrete defect or review finding. It
   owns attribution, invariant clustering, repair evidence, closure receipts,
   repeat-review admission, and review-loop stop rules.
5. Load repository-routed skills and add-ons required by the nearest `AGENTS.md`.

## Default Topology

```text
one stable writer
  -> one review wave covering the required independent dimensions
     -> correctness reviewer
     -> architecture/authority reviewer when material
     -> proof/test reviewer when material
  -> one consolidated repair by the current admitted writer
  -> one bounded closure wave
```

- Use only host-supported execution and continuation mechanisms.
- Apply repository concurrency policy; parallelize only independent, read-only
  work over a stable diff.
- Preserve reviewer identity and prior evidence when the host supports it;
  otherwise attach the same bounded packet to the closure reviewer.
- Do not duplicate delegated work in the parent session.
- Do not review a changing diff. Finish the writer slice before launching review.
- Parallelize independent read-only preparation while a writer works; do not
  allow concurrent writers to touch the same owner or files.

## One Slice

1. Freeze the authority, acceptance, write scope, forbidden effects, and proof
   commands for one reviewable slice.
2. Give one writer the complete slice. Name the authorized execution owner for
   focused checks before review; partial exposed behavior is invalid.
3. Launch one review wave only after the diff is stable.
4. Before launch, freeze the candidate digest and attach current required
   command receipts, architecture/currentness evidence, changed-file line
   budgets, and external ledgers. Do not make final review the first execution
   of an already-declared deterministic gate.
5. Require reviewers to report concrete authority-grounded findings or `PASS`.
   Reviewers must not redesign architecture, expand the plan, or add preferences.
6. Wait for the complete wave. Consolidate findings by canonical owner and
   invariant through `close-review-findings`, deduplicating symptoms and
   conflicting suggestions.
7. Return one findings packet to the admitted writer. Repair only findings that
   remain inside its accepted node and write scope, in one pass. Classify every
   other finding through `review-authority`; do not absorb new work.
8. Run one closure wave against the same authority and affected proof. Reuse a
   reviewer continuation only when the host supports it.
9. Close only when required correctness, architecture/authority, and proof gates
   pass on the same stable diff.

## Finding Packet

Give the repair writer one bounded packet:

```text
authority clause
canonical owner and invariant
reachable failure
exact evidence paths/lines
all equivalent cases and boundaries
required regression and deliberate mutation
forbidden effects
required focused and full gates
```

Exclude reviewer preferences, duplicate symptoms, speculative hardening, and
architecture changes without explicit authority.

## Build and Test Nodes

- Schedule only frozen work nodes. A coordinator may order dependency-ready
  nodes but may not create, merge, or expand them.
- Keep production and test writes in their admitted node scopes. A finding that
  crosses node ownership, scope, or acceptance is a classification event, not a
  repair instruction.
- Review a settled node before its dependent node starts. Final integration
  review covers the declared convergence node, not a newly discovered scope.

## Review Wave Selection

- Trivial localized change: one independent correctness reviewer.
- Non-trivial behavior change: correctness + proof reviewers.
- Architecture, ownership, schema, persistence, security, concurrency, or
  workflow/API change: correctness + architecture/authority + proof reviewers.
- Test-only repair: test reviewer plus one independent code reviewer when the
  oracle can weaken production proof.

Use repository-selected specialist reviewers when available. A `DRIFT PASS` is
local audit vocabulary, not a new product/runtime gate; do not add it to code or
architecture unless the governing plan explicitly requires it.

Assign those dimensions explicitly. Multiple reviewers with the same generic
prompt do not establish independent correctness, authority, and proof coverage.

## Anti-Loop Rules

- Do not launch a new review after each individual fix, test, or mutant.
- Do not launch reviewers while the writer is still editing.
- Do not replace malformed reviewer output repeatedly. One retry is allowed;
  after a second malformed result, replace that reviewer once or record degraded
  coverage.
- Do not reopen a closure wave for wording, naming, or count-only evidence when
  production behavior and the governing proof are unchanged; verify it in the
  existing proof session.
- Before another repair wave for the same owner cluster, complete the
  `close-review-findings` closure map and explain why the previous repair was
  incomplete.
- Stop the review loop when a later wave mostly rediscovers incomplete repairs,
  repair causes further findings, required architecture evidence is stale, or
  the mutation matrix is missing. Follow `close-review-findings`: report the
  open invariant instead of launching another microcycle.

## Parallelism

Safe parallel work:

- independent read-only API or architecture research;
- review dimensions over one stable diff;
- independent proof commands that do not mutate shared outputs;
- preparation for a later slice that does not edit current owner files.

Unsafe parallel work:

- multiple writers in the same owner or files;
- reviewers reading while the writer edits;
- tests sharing mutable build output without isolated snapshots;
- implementation of a later phase before the preceding exit gate;
- review agents selecting architecture or changing the plan.

## Closure Receipt

Report once per slice:

```text
slice and stable revision/diff
writer session
reviewer sessions and coverage
consolidated finding clusters and dispositions
focused, full, mutation, and runtime receipts
architecture/authority result when required
remaining risks or deferred behavior
```

Green tests alone do not close review. Reviewer prose alone does not grant
authority. Closure requires current attributable evidence on one stable diff.
