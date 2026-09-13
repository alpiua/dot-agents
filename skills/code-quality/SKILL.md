---
name: code-quality
description: Route non-trivial code changes to repository-owned quality, typing, modularity, model-hardening, and verification requirements.
---

# Code Quality

Use this skill before a non-trivial production, schema, configuration, or test
change. Read the repository root and owning package/service `AGENTS.md`, then
load `.agents/skills/code-quality/ADDON.md` when the repository supplies one.

## Core Procedure

1. Read the repository's canonical code-quality, typing/boundary, and
   verification documentation before implementation.
2. Keep types explicit at public, persistence, payload, parsing, and service
   boundaries. Prefer precise models, `TypedDict`, `Protocol`, unions, and
   validated narrowing over `Any`, unstructured generic dictionaries, casts, or
   type-ignore escapes.
3. Preserve one canonical owner per behavior. Split oversized or mixed-owner
   modules at real behavior/authority seams; do not hide a monolith behind a
   facade, generic helper, wildcard re-export, or mechanical chunks.
4. Apply the repository's boundary-model hardening rules to changed models.
5. Before review, enumerate every changed production and test file and verify
   its exact line count against repository and current Contract limits. A broad
   architecture suite that does not inspect the changed set is not this proof.
6. Run the smallest proof that can fail the change, then the required lint,
   type, architecture, backend, and release lanes. State the exact lane run;
   a lower-lane green result is not proof of a higher lane.

## Review

Review changed code for type escapes, missing boundary validation, invalid
module ownership/split, public import regressions, model hardening omissions,
and proof weaker than the changed seam. Report repository-policy violations with
the canonical document and exact source location.
