# Author Contract-Seam Tests

Write the narrowest deterministic test that proves the chosen contract scenario.

For `seam-test-first`, the independent author writes the admitted observable
Test Contract before production Build and records an expected-red receipt. If
the test is green before Build, record that the contract gap is already closed
or the behavior is already present; do not alter the contract to force red.
Freeze the reviewed diff as test-only before production implementation.

1. Enter through the supported public client, service, command, or handler.
2. Traverse real production collaborators to the contract boundary.
3. Use a protocol-faithful stub only for a genuinely unavailable dependency;
   do not replace the owner or behavior under proof with a mock.
4. Exercise both the accepted operation and a meaningful rejected or boundary
   operation when the contract includes a safety invariant.
5. Assert the outcome at the consumer boundary and durable state after the
   operation: values, current revision, revision count, state transition, or
   absence of mutation after rejection.
6. Exercise every material equivalent from the evidence map, including alternate
   production callers or backends when the contract promises parity.
7. Keep edits inside repository-declared test and fixture paths. Stop if proving
   the seam requires production, contract, adapter, or migration changes.

Do not call private helpers directly, bypass constructors with object allocation,
inject private collaborators, or replace production behavior with a lambda when
claiming contract-seam evidence. Such a test may be retained only when clearly
classified as `local-projection`.

For `deferred-final-test`, do not claim that a test failed before the production
change unless a recorded pre-fix replay proves it. Its normal proof is current
behavioral coverage plus the recorded sensitivity check; it must not manufacture
historical red.
