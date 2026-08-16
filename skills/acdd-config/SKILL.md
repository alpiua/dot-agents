---
name: acdd-config
description: Inspect and explicitly change ACDD Kilo gate-to-agent/model bindings.
---

# ACDD Configuration

Use this skill only after `/acdd settings` shows the effective configuration or
the operator explicitly requests an ACDD role/model change.

1. Read `.acdd/kilo.yaml`, the relevant native Kilo agent definitions, and
   `/acdd settings` output.
2. State the requested gate, role, agent, effective model, scope (repository or
   global), and configuration provenance before editing.
3. Prefer changing the repository adapter role binding. Change a native agent
   model only when the operator intends its global default to change across
   repositories.
4. Keep resolution order: adapter `role.model` override, native `agent.model`,
   then Kilo default only for coordinator roles.
5. Reject missing agents/models, primary-only agents selected for a subagent
   gate, and verifier/reviewer/test-author identity collisions.
6. Make the smallest explicit edit, validate the adapter and effective matrix,
   then report the changed source and resulting provenance.

Do not silently change provider credentials, global defaults, permissions, or
the runtime plugin. Do not use configuration changes to weaken independent
verification or review requirements.
