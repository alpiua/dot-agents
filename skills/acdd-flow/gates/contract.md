# Contract Gate

Turn the approved design into an executable, reviewable authority.
Owner: `acdd-flow` procedure. A host may require independent verification and
digest-bound admission after this gate before Build.

1. Prove every required change belongs to one bounded subtask.
2. Give every subtask explicit reads, writes, dependencies, acceptance, owner,
   producer/consumer relationships, and deferred downstream boundaries.
3. Check critical path, owner, backend, and authority premises against the live
   tree before freezing them.
4. Bind the proof policy, proof author, timing, observable outcome, sensitivity
   method, and post-fix invariant for every accepted claim. Do not assume a
   test-first method or deferred test authoring without an explicit policy.
5. For every new semantic, owner, interface, persistence, security, concurrency,
   migration, compatibility, or scope decision, cite an explicit user decision,
   owning plan, or existing Contract. Verifiers produce evidence, not authority:
   if no citation exists, emit `AUTHORITY_GAP` and stop or revise; do not encode
   a verifier recommendation.
6. Independently verify completeness, chain coverage, proof strength, and safe
   parallelism against the live task and implementation roots.

On verification failure, correct the same open candidate and repeat the
verification. Do not create a replacement task merely to carry a finding.

After a pass, freeze the authority surface: subtask identifiers, reads, writes,
dependencies, acceptance, contract matrices, and approved binding details.
The freeze also binds claim IDs, proof policy, required seams, and authority
digest.
Status, timing, evidence notes, and ordinary progress records are not frozen.
After freeze, only an explicit owner decision may add or replace scope.
