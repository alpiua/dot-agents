---
name: acdd-flow
description: Run portable authority-driven delivery gates for planned repository work.
---

# ACDD

Use this skill for planned delivery work that needs an explicit authority,
bounded implementation scope, independent review, and an honest handoff.
It is host-neutral. The host owns sessions, subagents, worktrees, tool calls,
evidence storage, and technical enforcement.

## Routing

1. Read the task or plan, its current gate state, and the repository context.
2. Read the host-selected repository adapter under `.acdd/` when it exists. For
   Kilo, use `.acdd/kilo.yaml`. An adapter may bind commands, paths, proof
   policy, risk checks, and guidance references; it must not encode host
   session lifecycle or weaken this skill's authority or evidence rules. The
   adapter is the only repository policy surface for this skill; do not create
   an `ADDON.md` for `acdd-flow`.
3. Load only the section for the current gate:
   `gates/design.md`, `gates/contract.md`, `gates/build.md`,
   `gates/review.md`, or `gates/handoff.md`.
4. Persist only the gate conclusion and durable evidence references required by
   the host. Do not preserve scratch reasoning or duplicate raw host records.

## Stage Map

Portable gates, in order:

1. Design
2. Contract
3. Build
4. Review
5. Handoff

A host plugin may insert independent verification and digest-bound admission
inside/after Contract before Build. Those host steps are not implemented by
this shared skill. For Kilo, see the repository adapter `guidance.workflow`
document for the numbered host sequence.

## Invariants

- Keep one canonical owner for each behavior, state, interface, and authority.
- Bound every subtask with reads, writes, dependencies, acceptance, and
  deferred downstream boundaries.
- Do not start production implementation before the current contract is
  independently verified and admitted by the configured host.
- A failed contract verification changes the open candidate, then re-verifies
  it. It is not a new task or an implementation blocker by itself.
- A passed contract freezes the initial Build baseline. Record scope revisions
  with the canonical `templates/evidence.md` Scope Revision row. Additive
  expansion does not reopen or re-admit the Contract; material drift requires
  the operator-decision reference required by that row.
- Reviewers and verifiers produce evidence; they are never authorities for
  architecture or Contract decisions. Every new semantic, owner, interface,
  persistence, security, concurrency, migration, compatibility, or scope
  decision must cite an explicit user decision, owning plan, or existing
  Contract. If none exists, emit `AUTHORITY_GAP` and stop or revise; do not
  encode a reviewer recommendation. Review may identify a gap or propose
  options, but cannot select one. A passed review cannot amend frozen Contract.
- Keep repairs inside the active authority when they do not change acceptance
  or ownership. Do not restart Contract merely because an in-scope test or
  review repair was needed.
- Independent review evaluates the settled implementation against the frozen
  authority. It cannot silently delete or narrow a required, deferred, or
  separately owned surface.
- A gate passes only from current, attributable evidence. Prose claiming a
  result is not evidence.
- Bind evidence to the current authority and input digest, affected paths,
  accepted claims, boundaries, review dimensions, producer role, and dependent
  evidence. A repair invalidates intersecting evidence and dependent seams;
  unrelated evidence remains current.
- Delegated helpers may produce candidate evidence but cannot grant official
  gate admission or impersonate the configured owner, verifier, or reviewer.
- Preserve every raw review result. Malformed output remains a blocking
  residual. Synthesis reconciles rather than votes: each finding has exactly
  one disposition, and every mutation has an auditable conflict record.
- Do not hand off while required checks, review repairs, or material blockers
  remain open.

## Risk Escalation

Require an explicit operator decision before finalizing a change to a public
interface, persisted representation, authorization or security rule,
concurrency behavior, migration, compatibility promise, or ownership boundary.
The repository adapter may classify additional risks or require more reviewers.

## Proof Policy

Contract must select one proof policy before Build:

- `incremental-test`: focused regression proof may be authored during Build;
- `seam-test-first`: before Build, an independent test author implements and
  reviews an executable production-seam test from an admitted observable test
  contract. Builders may run it but must not write it. Capture an expected-red
  receipt before implementation; a pre-Build green result is a Contract gap or
  already-present behavior, not proof of the new work;
- `deferred-final-test`: builders may run existing tests diagnostically but new
  regression tests are authored independently with `test-contract-seam` after
  implementation review convergence;
- `non-test-proof`: each accepted claim names an observable deterministic proof
  and why test authoring is inapplicable.

The adapter may select a default, but the admitted Contract binds the effective
policy. Build cannot silently change it. `seam-test-first` includes its test
contract in the same Contract review; do not create a separate approval gate.
Every policy requires observable
behavior, fail-closed assertions where relevant, and a sensitivity check by
pre-change replay, controlled mutation, fault injection, or an equivalent
deliberate break.

## Host Contract

The host must provide the following capabilities or record a visible degraded
mode and use an approved fallback:

- execute repository-bound deterministic checks with explicit argv, cwd,
  timeout, bounded output, and expected outcomes; command evidence must not
  infer success from model prose;
- identify the settled source revision and changed paths;
- keep producer, verifier, and reviewer roles independent where required;
- attach evidence to the task, plan, or host run without mutable replacement;
- prevent or reject writes outside the admitted scope when enforcement exists.

Required capability absence blocks the affected gate. Degraded mode is valid
only for a capability the adapter explicitly marks optional, and must record
the fallback and review escalation.

Kilo is one possible host. Kilo adapters may use child sessions, structured
messages, worktrees, permissions, diffs, and parent metadata. Those details do
not belong in this shared skill.

## Templates

Use `templates/task-contract.md` for task-level authority and
`templates/evidence.md` for compact gate conclusions and the canonical Scope
Revision row. Repositories may use an equivalent schema if it preserves the
same semantics.
