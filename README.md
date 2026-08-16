# Global Agent Skills

This directory contains reusable skills discovered globally by the agent
runtime. Repository-specific instructions, adapters, and workflows belong in
the owning repository's `.agents/` and `.acdd/` directories.

## Installed skills

| Skill | Purpose |
|---|---|
| `acdd-flow` | Runs authority-driven delivery gates: design, contract, build, review, and handoff. |
| `acdd-config` | Inspects or explicitly changes ACDD Kilo role-to-agent/model bindings. |
| `agent-instructions` | Creates and maintains `AGENTS.md`, local skill add-ons, and workflow guidance. |
| `close-review-findings` | Diagnoses defects and remediates or evidence-closes review findings. |
| `code-research` | Researches open-source repositories to understand implementation patterns. |
| `codebase-memory` | Uses the codebase knowledge graph for structural search, tracing, and impact analysis. |
| `commit-workflow` | Prepares intentional, reviewable Git commits after explicit authorization. |
| `explore` | Searches a codebase for a targeted question. |
| `feature-research` | Researches existing architecture before implementing a complex feature. |
| `find-skills` | Finds or installs a skill for a requested capability. |
| `planner` | Manages Planner task, roadmap, and evidence workflows. |
| `test-contract-seam` | Authors independent contract-seam tests under the admitted proof policy. |
| `test-suite-refactor` | Refactors typed test suites without weakening production-path coverage. |
