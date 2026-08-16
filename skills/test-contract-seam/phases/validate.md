# Validate Test Evidence

1. For `seam-test-first`, verify the independent author, reviewed frozen
   test-only diff, and expected-red receipt before production Build. For
   `deferred-final-test`, verify implementation review convergence first; never
   manufacture historical red.
2. Run the new or changed contract-seam tests and their adjacent tests.
3. Run repository-required integration, quality, and full-suite checks.
4. Confirm the test uses production behavior across the stated boundary rather
   than a mock that returns the expected answer.
5. Confirm the asserted state and failure semantics match the settled contract.
6. Confirm the final source revision matches the reviewed inputs.
7. Run the named sensitivity check for each product-contract scenario and prove
   the test rejects the deliberate contract break.
8. Inspect the final diff and verify it contains only admitted test, fixture, and
   evidence paths.
9. Obtain independent inspection of the complete test diff. The delivery author
   cannot self-accept deferred final-test evidence.
10. Record each scenario's evidence class, test location, commands, result, and
   any limitation in the evidence map.

If validation exposes a production defect, stop closing test evidence. Return
the defect to the owning delivery work, preserve the test and finding, then
refresh the affected review and test evidence after repair.

Any uncovered accepted claim, stale implementation revision, failed sensitivity
check, unauthorized path, or missing independent inspection blocks completion.
