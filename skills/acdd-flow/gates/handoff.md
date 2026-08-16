# Handoff Gate

Close delivery with an accurate repository and planning state.
Owner: `acdd-flow` procedure; closure checks come from the repository adapter
when declared.

1. Confirm the final review applies to the current settled inputs.
2. Confirm required runtime, integration, quality, and derived-artifact checks
   passed with their expected outcomes. When the adapter declares
   `commands.handoff`, the host must execute those exact argv receipts.
3. Confirm every accepted claim has current test coverage or justified non-test
   proof, every required seam is current, and no active repair, malformed review
   residual, or material blocker remains.
4. Under `deferred-final-test`, require review convergence, a full claim-to-
   boundary coverage map, independent regression authoring, and one sensitivity
   demonstration before Handoff. A production defect returns to its owning
   builder and invalidates only intersecting review/test evidence.
5. Update the owning task or plan with the completed scope, deferred work,
   evidence references, and known operational follow-up.
6. Run the repository-specific closure procedure.

Pass only when handoff reports no unresolved blocker. Publishing reports or
applying a change is a host or repository operation, not an implicit gate side
effect.
