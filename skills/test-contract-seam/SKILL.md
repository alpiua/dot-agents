---
name: test-contract-seam
description: Author and strengthen independent contract-seam tests under the admitted proof policy.
---

# Contract-Seam Testing

Use this skill under the admitted proof policy. It writes or strengthens tests
as evidence for observable product behavior, not as a way to mirror private
implementation details.

For `seam-test-first`, run an independent test author after admission and before
production Build. The author implements the admitted observable Test Contract,
then submits a reviewed, frozen, test-only diff and an expected-red receipt.
The receipt records the expected-red result; if the pre-Build run is green, it
records that the contract gap is already closed or the behavior is already
present. Green is not a reason to weaken the contract or manufacture a failure.

For `deferred-final-test`, use an author independent from the delivery author.
Author after implementation review converges. Do not manufacture historical red
for a test written after implementation. The delivery author may run existing
tests, but must not author or weaken the new tests. If the host cannot provide
role separation, block unless the admitted Contract and repository adapter
explicitly mark separation optional and authorize a degraded mode. That mode
still requires independent review of the complete test diff.

## Admission

Start only when all inputs are present:

- settled contract and accepted claim IDs;
- implementation revision and changed-path set;
- converged implementation review with no unresolved material finding;
- proof policy and repository adapter;
- existing test inventory and required production seams.

Missing or stale input blocks test authoring. Do not infer coverage from a green
suite or reconstruct historical evidence.

## Routing

1. Read the frozen or settled contract, current source revision, changed paths,
   review findings, and adjacent tests.
2. Read `.agents/skills/test-contract-seam/ADDON.md` when it exists. It may add
   test commands, supported integration fixtures, repository boundaries, and
   stricter requirements; it must not weaken this skill's evidence rules.
3. Load `phases/discover.md`, then only the needed sections of `author.md`,
    `strengthen.md`, and `validate.md`.
4. Keep the evidence map and final conclusion. Do not manufacture a historical
   failing-test claim for a test written after implementation.

## Core Rules

- A product-contract test proves an observable scenario across the completed
  production seam. It does not only prove that one function called a mock.
- For every relevant test, state the user or contract guarantee that a change
  making the test fail would violate. If the answer is only a current internal
  algorithm, a mock invocation, or a private method, do not count it as a
  regression test: reclassify it, rewrite it through the appropriate seam, or
  remove it when it has no justified supplemental value.
- Prefer real in-process collaborators and stateful stores. Use a protocol-
  faithful stub only at an unavailable external boundary.
- Assert observable outputs, persisted state, revisions, invariants, and
  rejected-operation non-mutation where they are part of the contract.
- Classify each test by evidence strength. A local projection test or an
  architecture guard is useful supplemental coverage, not runtime proof.
- Select the required evidence level before authoring: static contract,
  component, in-process transport, live persistence/process, or cross-service.
  Local projection and architecture guards cannot replace a required transport,
  transaction, storage, process, restart, or producer-consumer proof.
- An Integration Test Contract must name the real environment and readback
  boundary it proves. Use production parsers, adapters, servicers, interceptors,
  clients, or stores; mock/helper-only proof cannot satisfy that contract. Place
  fakes only behind unavailable external boundaries.
- Keep tests deterministic and scoped to the declared scenario. Do not rewrite
  unrelated suites to improve a metric or a style preference.
- Write only tests, test fixtures, and the host-owned evidence map. Production,
  contract, adapter, and migration edits return to the owning delivery role.
- Name at least one plausible contract-breaking mutation per scenario and prove
  that the final test detects it by controlled mutation, pre-fix replay, fault
  injection, or an equivalent deliberate break.
- Cover the accepted, rejected, malformed, boundary, persistence/restart, and
  alternate-caller dimensions that are material to the settled contract. Record
  every inapplicable dimension with a reason.
- A strong integration seam may subsume weaker tests, but only after preserving
  every unique negative, cleanup, persistence, restart, concurrency, and
  alternate-caller contract. Record `old test -> retained seam test -> preserved
  dimensions` and delete the weaker test in the same test-only diff. Do not keep
  both. Do not add a contract-seam test whose observable contract is already
  proven. When invoked from `test-suite-refactor`, author only missing
  dimensions, then return to that skill for `REMOVE`.
- If a new test reveals a production defect, return the defect to its owning
  delivery work. Preserve the failing test and evidence; do not repair production
  code or weaken the test from the test-author role.
- After a production repair, re-run affected delivery checks and independent
  implementation review before treating the test evidence as current. Then
  refresh the test evidence and independently inspect the test-only diff.

## Evidence Classes

| Class | Purpose | Counts as product-contract proof |
| --- | --- | --- |
| `contract-seam` | Observable behavior through a completed production seam | Yes |
| `integration-boundary` | Protocol, serialization, persistence, or adapter behavior | Yes, when it covers the scenario |
| `local-projection` | Local mapping, dispatch, or calculation | No; supplemental |
| `architecture-guard` | Structural constraint or caller inventory | No; supplemental |
| `brittle-structural` | Private internals or organization-sensitive assertions | No; refactor candidate |

Required integration evidence that is skipped, unavailable, or replaced by a
mock above the claimed boundary blocks completion.

## Host Boundary

The host provides sessions, role separation, filesystem isolation, test
execution, evidence storage, and any write permissions. Kilo may use child
sessions, diffs, structured messages, and dynamic permissions; those runtime
details do not belong in this shared skill.

## Output

Use `templates/evidence-map.md` to record scenario coverage, test classification,
named mutations, commands, author/reviewer roles, unresolved defects, and the
replacement map. The output must make clear which tests prove product behavior,
which are only supplemental guards, and which weaker tests were deleted. Any
uncovered accepted claim, failed sensitivity check, or subsumed test kept
alongside its replacement blocks completion.
