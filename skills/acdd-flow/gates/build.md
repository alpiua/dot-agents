# Build Gate

Implement admitted work while preserving contract authority.
Owner: `acdd-flow` procedure; builder role and write bounds come from the
repository adapter and admitted Contract.

1. Confirm a current, independently verified Contract before the first
   production edit for each admitted subtask.
2. Confirm dependencies are accepted and no parallel write or resource conflict
   exists.
3. Trace the reachable scenario through the canonical owner, consumer,
   persistence or external boundary, and failure behavior.
4. Implement the smallest fitting change and follow the Contract's proof
   policy. Under `incremental-test`, produce the focused regression during
   Build. Under `deferred-final-test`, do not author or weaken tests; record the
   candidate and coverage inputs for the independent final test author. Under
   `non-test-proof`, run the bound deterministic proof.
5. Run the repository's integrated runtime and quality checks on the settled
   tree. When the adapter declares `commands.build`, the host must execute those
   exact argv receipts rather than inferring success from prose.

Use one delivery unit per independently schedulable subtask. A host may run
independent units in parallel only after it proves their declared writes and
resources do not conflict.

When accepted claims cross delivery-unit boundaries, stop writers, freeze one
candidate revision, and require connected-cohort seam evidence before final
review. Helpers create no official ACDD state.

Fix an in-scope defect directly, record a short conclusion, and rerun focused
checks. For an approved scope expansion, use the canonical Scope Revision row
in `templates/evidence.md`; do not duplicate its fields or semantics here.

Under `deferred-final-test`, Build hands Review the accepted claims, production
seams, existing-test inventory, and candidate sensitivity methods. It does not
pre-author the final tests.
