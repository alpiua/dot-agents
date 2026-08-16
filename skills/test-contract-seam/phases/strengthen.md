# Strengthen Adjacent Tests

Refactor only tests in the changed or directly adjacent contract scope.

## Reclassify

Mark a test `local-projection` when it supplies expected mock output and asserts
that production code used expected arguments. It proves a local mapping or
dispatch decision, not the mocked system's behavior.

Mark a test `architecture-guard` when it scans callers, imports, registrations,
or file inventory. It can prevent an unregistered structural path but does not
prove runtime behavior.

## Replace Or Retain

Replace a `brittle-structural` test when it depends on private fields, private
helper monkeypatches, constructor bypasses, exact internal inventory, or other
details that can change without product behavior changing.

Replacement must preserve every observable scenario the old test covered. Run
the old and replacement scope before removing the old assertion; uncertainty
means retain and reclassify, not delete.

Retain a useful local or structural test when it is cheap, stable, and explicitly
classified. Do not delete it merely because it cannot close a product-contract
scenario. Add a separate contract-seam test where the contract needs proof.

Do not broaden this work into a suite-wide cleanup. Every changed test needs a
named contract scenario, adjacent regression risk, or explicit repository rule.
