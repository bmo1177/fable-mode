---
name: changelog
description: "Version history for the fable-mode skill"
---

# Changelog — Fable Mode

All notable changes to the fable-mode skill will be documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
