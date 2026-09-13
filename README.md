# Global Agent Skills

This directory contains reusable skills discovered globally by the agent
runtime. Repository-specific instructions, adapters, and workflows belong in
the owning repository's `.agents/` and `.acdd/` directories.

## Installed skills

| Skill | Purpose |
|---|---|
| `acdd-flow` | Routes planned delivery to contract planning, delivery orchestration, review authority, and handoff. |
| `acdd-kilo-config` | Inspects or explicitly changes Kilo ACDD role-to-agent/model bindings. |
| `agent-instructions` | Creates and maintains `AGENTS.md`, local skill add-ons, and workflow guidance. |
| `close-review-findings` | Diagnoses defects and remediates or evidence-closes review findings. |
| `code-research` | Researches open-source repositories to understand implementation patterns. |
| `codebase-memory` | Uses the codebase knowledge graph for structural search, tracing, and impact analysis. |
| `commit-workflow` | Prepares intentional, reviewable Git commits after explicit authorization. |
| `django-gridviewspec` | Architecture standards, lifecycle invariants, Islands Architecture (GridViewSpec + HTMX + Alpine), and MCP tooling for Django UI. |
| `documentation-edits` | Writes docs, skills, AGENTS.md, and comments as short present-tense instructions. |
| `explore` | Searches a codebase for a targeted question. |
| `feature-research` | Researches existing architecture before implementing a complex feature. |
| `find-skills` | Finds or installs a skill for a requested capability. |
| `planner` | Manages Planner task, roadmap, and evidence workflows. |
| `test-contract-seam` | Authors independent contract-seam tests under the admitted proof policy. |
| `test-suite-refactor` | Refactors typed test suites without weakening production-path coverage. |
