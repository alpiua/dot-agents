---
name: planner
description: Use for portfolio plans, roadmaps, active-plan state, cross-repository dependencies, and MemPalace indexing of plans or documentation.
---

# Planner

Use for portfolio plans, roadmaps, active-plan state, cross-repository
dependencies, and MemPalace indexing of plans or documentation.

## Route

1. Load the current repository's `.agents/skills/planner/ADDON.md`.
2. Resolve the central Planner Git root, repository id, plan namespace,
   documentation allowlist, MemPalace wing, and Linear policy from the add-on.
3. Read Planner `PORTFOLIO.md`, the active-plan links in
   `CURRENT_EXECUTION.md`, and the selected plan.
4. Keep product code, tests, review, staging, and commits in the owning Git
   repository. Keep portfolio plans and task state in the Planner Git repository.
5. Split cross-repository work into owner-specific tasks with explicit contract
   and evidence handoffs.

## MemPalace

- Use wing `planner-<repository>` from the add-on.
- Planner plan lifecycle maps to rooms `plans-active`, `plans-backlog`, and
  `plans-complete`.
- Owning-repository documentation uses that checkout's MemPalace room taxonomy
  (`documentation`, `docs`, or `general`). Wing isolation is mandatory; room
  names may differ by repository.
- ContextUnity roadmap maps to room `roadmap`.
- Never index source, tests, generated docs, caches, build output, secrets, or
  files from another repository into the wing.
- Use changed-set sync for exact files. Full initialization may mine only the
  declared plan directory or documentation directories.

## Independent Checkouts

- Treat the implementation repository checkout and Planner checkout as separate
  Git roots. Do not depend on an enclosing workspace router.
- When isolated writes are required, create independent worktrees from their
  owning repositories: `git worktree add <implementation-path> -b <branch>` and,
  separately, `git worktree add <planner-path> -b <planner-branch>`. Do not put
  Planner and implementation changes in one worktree or one commit.
- Resolve Planner from `CONTEXTUNITY_PLANNER_ROOT` when set; otherwise use the
  local add-on's default relative path.
- Sync plans with `project_dir=<planner checkout>` and docs with
  `project_dir=<implementation checkout>`. Submit separate changed sets, both to
  the add-on's wing.
- Search only `wing=planner-<repository>` for that repository's plans/docs.

## Boundaries

- Linear is allowed only when the add-on says `linear: true`.
- Do not activate codebase-memory or pi-review-agents merely because Planner is
  used. Repository-local instructions may select their own tools.
- A MemPalace result is navigation evidence, not plan or runtime authority.

## Done

Planner state names the owner repository; implementation evidence comes from
that Git root; the correct MemPalace wing is refreshed; no unrelated wing or
repository was indexed.
