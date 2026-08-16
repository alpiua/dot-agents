# Review Gate

Independently evaluate the settled result, not the implementation process.
Owner: `acdd-flow` procedure; reviewer set comes from the repository adapter.

1. Give reviewers the settled source revision, changed scope, frozen authority,
   and required review dimensions.
2. Keep reviewers independent from the delivery author. The repository adapter
   (`.acdd/<host>.yaml`) roles and risk settings classify the reviewer set and
   special dimensions.
3. Preserve every raw reviewer result before interpretation. Malformed output
   remains a blocking residual. Map every finding exactly once to a disposition;
   synthesis does not vote, and every normalized or changed field requires an
   auditable conflict record.
4. Treat a finding that conflicts with frozen required scope as `invalid` or
   `plan-deferred`; do not silently remove the required behavior.
5. Reviewers produce evidence, not architecture or Contract authority. Every
   new semantic, owner, interface, persistence, security, concurrency,
   migration, compatibility, or scope decision must cite an explicit user
   decision, owning plan, or existing Contract. If absent, emit `AUTHORITY_GAP`
   and stop or revise; do not encode a reviewer recommendation. Review may
   identify a gap or propose options, but cannot select one; a pass cannot amend
   frozen Contract.
6. Bind review evidence to candidate digest, paths, claims, boundaries, and
   dimensions. When a repair intersects any of them, invalidate that evidence
   plus dependent seams, rerun affected Build checks, and independently review
   the changed settled result. Preserve unaffected evidence.

Under `deferred-final-test`, first converge implementation review, then invoke
`test-contract-seam` with an independent author. A discovered production defect
returns to Build and invalidates intersecting review evidence. After test
evidence passes sensitivity checks, independently inspect the complete test-only
diff.

Pass only when all required dimensions were reviewed, every accepted claim has
its policy-bound proof, and no unresolved material finding remains.
