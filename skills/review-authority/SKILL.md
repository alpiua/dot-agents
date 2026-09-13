---
name: review-authority
description: Govern review and repair findings so only current task authority can change code, scope, contracts, or acceptance.
---

# Review Authority

Use this skill whenever a reviewer, verifier, auditor, or repair loop can create
findings that may change code, tests, documentation, configuration, scope, or an
implementation contract.

This skill owns the universal authority boundary for review and repair. Delivery
workflows may map its outcomes into host-specific verdicts, but must not duplicate
or weaken these rules.

## Authority

Apply authority in this order:

1. Explicit current user decision.
2. Current admitted Contract or equivalent bounded task authority.
3. Current owning plan or specification.
4. Reviewer findings and suggestions.

A reviewer supplies evidence. A reviewer is not authority for a new owner,
interface, capability, persisted representation, security rule, concurrency
behavior, compatibility promise, dependency, acceptance condition, or scope.

## Finding Admission

A finding may block current work only when it identifies a concrete violation
of at least one current authority:

- an accepted claim or observable requirement;
- a forbidden effect or containment boundary;
- an admitted dependency or integration seam;
- the current write or ownership boundary;
- an explicit quality or proof gate selected by the current Contract.

The finding must cite that authority and the exact candidate behavior or source
that violates it. Reviewer preference, speculative hardening, cleanup, possible
future use, or an unsupported best practice is not a blocking finding.

## Routing

Classify and route every admitted finding exactly once:

```text
required defect + current owner/write scope
  -> same author or builder repairs
  -> same stable reviewer verifies closure

required defect + different owner/write scope
  -> route to that owner
  -> current author must not modify it

suggestion without current authority
  -> DEFERRED
  -> no code, Contract, plan, or scope expansion

reviewer claim contradicts explicit authority
  -> return for withdrawal or authority-grounded restatement

missing, contradictory, or materially incomplete authority
  -> USER_DECISION_REQUIRED
  -> stop only the affected work
  -> agents must not select an architecture
```

A source mismatch with unambiguous authority is an ordinary defect. It does not
require a user decision when the owning work and write scope already admit the
repair.

## Repair

- Keep repair inside the current acceptance, ownership, and write boundaries.
- Resume the same author/builder and stable reviewer when the host supports it.
- Give the repair author only current consolidated authority-grounded findings.
- Do not use a repair to add adjacent improvements or reviewer suggestions.
- Do not silently widen reads, writes, dependencies, proof policy, or non-goals.
- If repair exposes missing authority, stop and return `USER_DECISION_REQUIRED`.

## Closure

A finding closes only when the same governing authority is satisfied by current
attributable evidence. A reviewer `PASS` cannot amend authority, approve an
unsupported expansion, or convert deferred work into completed work.

Return unresolved malformed evidence honestly. Do not reinterpret silence as
withdrawal or completion.

## Host Mapping

Hosts and workflows may rename transport verdicts while preserving semantics:

```text
DEFERRED               -> non-blocking recorded suggestion
USER_DECISION_REQUIRED -> authority/operator gate
```

For ACDD, map `USER_DECISION_REQUIRED` to `AUTHORITY_GAP` owned by `USER`.
Host mechanics, session topology, artifact schemas, model selection, and
repository paths do not belong in this skill.
