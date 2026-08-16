---
name: commit-workflow
description: Create intentional, reviewable Git commits in the current repository. Use when the user asks to stage, split, commit, amend, or prepare commits in a dirty worktree.
---

# Commit Workflow

Use this procedure only after the user explicitly authorizes a commit-related
action. The active repository's `AGENTS.md` defines required gates and hooks.

## Repository Routing

Read the nearest repository `AGENTS.md` first. After loading this base skill,
load `.agents/skills/commit-workflow/ADDON.md` when the repository routes to one.
The add-on may select repository hooks, formatting, validation, and commit
boundaries; it must not authorize a commit, weaken Git safety, or change another
repository's worktree.

## 1. Establish Scope

1. Read the repository's `AGENTS.md` and confirm its Git root.
2. Inspect the worktree before changing the index:

   ```bash
   git status --short
   git diff --stat
   git diff --cached --stat
   git log -10 --format='%h %s'
   ```

3. Treat every existing staged, unstaged, and untracked path as intentional.
   Do not restore, reset, delete, amend, rebase, or move unrelated work.
4. Commit one Git repository at a time. Cross-repository work requires separate
   commit plans and verification in each owning repository.

## 2. Design And Verify

1. Split commits by independently reviewable behavior, contract, or delivery
   outcome, not by file extension or arbitrary size.
2. Include the smallest relevant tests or verification with each behavior slice.
3. Run the repository-defined focused gates before staging. Do not invent
   ContextUnity-, Planner-, or package-specific commands in this shared skill.
4. Ask before any expensive, external, destructive, or deployment-like gate
   unless the user already authorized it.

## 3. Stage Safely

1. Prefer `git add -- <paths>`; do not default to `git add .`.
2. After staging each slice, inspect:

   ```bash
   git diff --cached --stat
   git diff --cached --check
   git diff --cached --name-status
   ```

3. Confirm no unrelated path entered the slice. Do not use `git reset --hard`,
   `git checkout --`, or worktree `git restore` to prepare history.
4. Do not use `git stash` in shared or multi-worktree repositories. Resolve
   ownership explicitly instead.

## 4. Commit And Confirm

1. Use a concise imperative subject that names the observable outcome.
2. Run normal hooks unless the user explicitly authorizes a documented exception.
3. After every commit, inspect:

   ```bash
   git show --stat --oneline HEAD
   git status --short
   ```

4. Report commit hashes, subjects, executed verification, and remaining
   worktree changes. Never claim a clean worktree unless `git status` proves it.
