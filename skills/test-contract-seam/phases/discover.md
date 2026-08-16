# Discover Test Evidence

Build the evidence map before writing a test. For `seam-test-first`, do this
after admission and before production Build; for `deferred-final-test`, do it
after implementation review converges.

1. Identify the contract scenario, canonical owner, production consumer, and
   persistence or external boundary.
2. State the observable success outcome, failure behavior, and invariant that
   must survive a rejected operation.
3. Inspect adjacent tests by changed path, owner, caller/consumer path, and
   existing scenario coverage.
4. For every relevant test, name the user or contract guarantee it preserves.
   If a failure would show only an internal algorithm, mock invocation, or
   private-method change, classify it as supplemental or select it for seam
   rewrite or removal; do not treat it as regression proof.
5. Classify every relevant test using the evidence classes in `SKILL.md`.
6. For each contract scenario, choose one action: retain, add, strengthen,
   reclassify, or remove a test.
7. Enumerate material dimensions: accepted, rejected, malformed, exact-boundary,
   alternate caller/backend, persistence/restart, and external-effect failure.
8. Name one plausible contract-breaking mutation for every scenario selected for
   product-contract proof. A test with no sensitivity target is not admissible.

Use this path form in the evidence map:

```text
accepted claim -> scenario -> canonical owner -> consumer -> boundary -> observable outcome
```

Do not infer a production seam merely from a unit-test mock. Read the production
path and identify the actual serialization, validation, persistence, transport,
or public-response boundary.
