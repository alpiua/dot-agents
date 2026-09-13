---
name: documentation-edits
description: >
  Write docs, ADRs, README, AGENTS.md, skills, and comments as the current
  system in present tense. Use on every docs or comment change, ADR edit, and
  after removing a subsystem.
---

# Documentation edits

Load before changing `docs/`, ADRs, README, `AGENTS.md`, skills, or comments.

Write what the system is now and what to do with it. Present tense.

Delete a file whose only subject is a removed system. Do not keep it as history, archive, or a status banner.

ADRs record the accepted end state only: decision, current shape, consequences, non-goals. Replace the decision when it changes. Do not narrate prior stacks or intermediate designs.

`AGENTS.md`, skills, and comments: one short imperative per rule. Owner path and verb (`Read`, `Write`, `Load`, `Use`, `Keep`).

`docs/` holds current architecture and runtime contracts. Planner holds task state. Skills hold procedure. Generated context dumps are not SSoT.

Do not write migration history. Do not justify a platform rule with one tenant vertical. Examples stay generic unless the name is a runtime type.

Validate: every sentence describes current runtime or an action; ADR Context has no former stack and no niche origin story; `git diff --check` in the owning repository.
