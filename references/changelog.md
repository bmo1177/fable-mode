---
name: changelog
description: "Version history for the fable-mode skill"
---

# Changelog — Fable Mode

All notable changes to the fable-mode skill will be documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [3.0.0] - 2026-07-17

### Added

- **Hash-Chained Audit Trail** — `.fable/trail/chain.audit.jsonl` with SHA-256 linking for tamper-evident evidence. Each entry has `prev_hash` and `this_hash`; chain verification command detects any byte modification.
- **Scope Guard** — `Planned Files` field in every stage plan. Blacksmith enforces scope: modifications outside the declared list are flagged as drift and reverted.
- **Construction-Time Validation** — Cartographer validates every verification check before plan acceptance. Checks must be machine-verifiable, able to fail honestly, and produce evidence.
- **Shadow Mode** — `FABLE_SHADOW=1` enables observation-only mode. All protocols run (gate, checks, proof, mirror) but nothing is enforced. Perfect for safe adoption.
- **Bounce Targets** — Each stage declares a target (`same`, `Stage N`, or `plan`) for re-entry on failure. Supports targeted recovery instead of always re-running the current stage.
- **Model-Tier Branching** — Frontier models (Opus, Sonnet 4.x) get coarse stages; local models (Haiku, Llama, Phi) get fine-grained stages with explicit acceptance criteria.
- **Wave-Based DAG Execution** — Stages grouped into barrier-synchronized waves. Resource conflict matrix prevents parallel modifications to the same file.
- **Adversarial Review** — Dual independent reviewer (`references/adversarial_review.md`) with no shared implementation context. Catches shared blind spots.
- **Status Badge System** — `STAGE STATUS: [PENDING / IN PROGRESS / PASS / FAIL / UNVERIFIED / BOUNCED]` in stage plan templates for visual tracking.
- **3 New Failure Paths** — F13 (bounce target chain too long), F14 (scope drift), F15 (audit trail integrity failure).

### Changed

- `SKILL.md` updated to v3.0.0 with v3.0 feature list, shadow mode quick start, model-tier selection, updated references.
- `agents/cartographer_agent.md` updated with scope declaration, bounce target definition, construction-time validation, model-tier assessment.
- `agents/blacksmith_agent.md` updated with scope guard enforcement, bounce target routing in failure handling.
- `agents/mirror_agent.md` updated with adversarial review protocol (dual reviewer + reconciliation).
- `references/failure_paths.md` expanded from 12 to 15 scenarios with three new detailed failure paths.
- `references/parallel_delegation_guide.md` updated with wave-based execution, resource conflict matrix, fan-out/fan-in patterns.
- `templates/stage_plan_template.md` updated with Planned Files, STAGE STATUS badges, Bounce Target, construction-time validation acceptance criteria.

### Added (New Files)

- `references/audit_trail.md` — Hash chain protocol, entry format, verification command, lifecycle integration.
- `references/shadow_mode.md` — Shadow behavior per agent, log format, adoption transition process.

### Version

- 3.0.0 (governance & verification expansion).

---

## [2.1.0] - 2026-06-24

### Added

- **Fresh-context verification** — Mirror and Sage can run as separate subagents with fresh context (no shared biases).
- **Structured memory system** — Cross-session learning in `.fable/memory/` with lessons, corrections, approaches, failures.
- **Progress grounding** — Every claim must cite specific tool evidence (no "should work").
- **Brevity instructions** — Prevent over-elaboration and unnecessary preamble.
- **Fallback handling** — Server-side refusals, client-side rate limits, manual escalation paths.
- **Task budgets** — Max iterations, max time, max API calls per stage (with escalation).
- **Send-to-user tool** — Communication with user during execution for decisions, clarifications.

### Changed

- SKILL.md updated with new Fable 5 integration patterns.

### Removed

- None.

### Version

- 2.1.0 (fresh-context verification, structured memory, progress grounding).

---

## [2.0.0] - 2026-06-24

### Added

- **3 new agents**:
  - `researcher_agent` — Research specialist with source verification, synthesis, contradiction detection.
  - `data_analyzer_agent` — Data specialist with schema validation, integrity checks, edge cases.
  - `deployer_agent` — Deployment specialist with environment checks, rollback, smoke tests.
- **Vision verification** — Screenshot comparison, design fidelity, accessibility checks.
- **Self-verification** — Write-your-own-tests, reasoning re-derivation, output-against-goal.
- **Dynamic re-planning** — Cartographer revises plans when evidence contradicts assumptions.
- **Long-session support** — Checkpoint system, context compaction, session resume.
- **Memory integration** — `.fable/` directory structure with context, plan, proof, delivery, memory.
- **Effort control** — Different thinking depth for different stages (high/xhigh for planning/review, medium/high for execution).

### Changed

- SKILL.md rewritten with 7-agent team, Mermaid diagrams, Fable 5 integration.
- `cartographer_agent.md` updated with dynamic re-planning and context budget management.
- `blacksmith_agent.md` updated with self-verification, effort control, fresh-context verification.
- `mirror_agent.md` updated with fresh-context verification and effort control.
- `README.md` rewritten with Mermaid diagrams, agent team section, Fable 5 integration.
- `verification_examples.md` expanded from 15 to 25+ examples (including vision & self-verification).
- `failure_paths.md` expanded from 12 to 15 failure scenarios.
- `gate_protocol.md` updated with long-horizon task detection.

### Removed

- None.

### Version

- 2.0.0 (3 new agents, vision verification, self-verification, dynamic re-planning, long-session support).

---

## [1.0.0] - 2026-06-23

### Added

- **Initial published release** of fable-mode as a standalone skill.
- **3 Modes**: `basic` (standard staged execution), `full` (with parallel delegation), `lite` (lightweight for smaller tasks).
- **4 Agent Definitions**:
  - `cartographer_agent` — Stage decomposition, check definition, plan production.
  - `blacksmith_agent` — Stage execution, verification check running, failure handling.
  - `sage_agent` — Final Proof of Deeds, cross-stage integration validation.
  - `mirror_agent` — Skeptical self-review, process integrity check.
- **6 Reference Documents**:
  - `check_types_by_domain.md` — Verification patterns for code, research, data, multi-step.
  - `gate_protocol.md` — When to enter/exit fable mode.
  - `failure_paths.md` — Complete failure path map (12 scenarios).
  - `parallel_delegation_guide.md` — How to dispatch independent stages.
  - `verification_examples.md` — 15+ concrete check examples with quality ratings.
  - `changelog.md` — This file.
- **3 Templates**:
  - `stage_plan_template.md` — Fillable stage plan format.
  - `final_proof_template.md` — Final proof checklist.
  - `delivery_summary_template.md` — Delivery summary format.
- **3 Examples**:
  - `multi_file_refactor.md` — Fable mode on a multi-file code refactor.
  - `api_migration.md` — Fable mode on an API version migration.
  - `research_project.md` — Fable mode on a research/writing project.
- **Enhanced SKILL.md**:
  - YAML metadata block (version, status, related_skills).
  - Quick Start section with command examples.
  - Mode Selection Guide with decision tree.
  - Agent Team table.
  - Orchestration Workflow diagram (4-rite pipeline).
  - Trigger Conditions with keyword lists.
  - Does NOT Trigger table.
  - Failure Paths summary table.
  - References directory listing.

### Version

- 1.0.0 (initial published release).
