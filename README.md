<p align="center">
  <br>
  <img src="https://img.shields.io/badge/version-1.0.0-blue" alt="Version">
  <img src="https://img.shields.io/badge/status-active-brightgreen" alt="Status">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  <img src="https://img.shields.io/badge/platform-OpenCode%20|%20Claude%20Code-lightgrey" alt="Platform">
  <br>
</p>

<h1 align="center">Fable Mode</h1>

<p align="center">
  <strong>Staged execution discipline for AI agents that refuses to let them work beneath their own standard.</strong>
</p>

<p align="center">
  Decomposes complex work into stages with machine-verifiable checks.<br>
  Enforces proof before delivery. Demands skeptical self-review.
</p>

---

## The Problem

AI agents are great at single-step tasks. But ask them to refactor 12 files, migrate an API, or write a research report — and things break silently.

- Stages get skipped because "it should work"
- Tests are never actually run
- Regressions pile up undetected
- The agent delivers with confidence, but no evidence

**Fable Mode fixes this.** It forces the agent to plan before acting, prove each stage before moving on, and review its own work before delivery.

---

## Features

| Feature | What It Does |
|---------|-------------|
| **Gate Check** | Refuses to activate on trivial tasks — no overhead where discipline isn't needed |
| **3 Modes** | `basic` (sequential), `full` (parallel delegation), `lite` (lightweight) |
| **4 Agents** | Cartographer, Blacksmith, Sage, Mirror — each with defined roles and boundaries |
| **Stage Planning** | Every task decomposed into stages with concrete outputs and verification checks |
| **Machine-Verifiable Checks** | Every check is a command that can pass or fail — no "looks good to me" |
| **Final Proof** | Validates that all stages integrate and requirements are met before delivery |
| **Self-Review** | Mirror agent catches hidden complexity, assumptions, and weakened checks |
| **Failure Paths** | 12 documented failure scenarios with recovery strategies |
| **Domain-Agnostic** | Works for code, research, data, and multi-step tasks |

---

## Quick Start

**Activate fable mode:**
```
fable mode on — refactor the authentication module across 5 files
```

**What happens:**
1. **Gate check** — Is this complex enough? Yes → proceed
2. **Cartographer** — Decomposes into 5 stages with verification checks
3. **Blacksmith** — Executes each stage, runs checks
4. **Sage** — Validates the whole holds together
5. **Mirror** — Skeptical self-review before delivery

**That's it.** The agent delivers with evidence, not confidence.

---

## Installation

Fable Mode is an [OpenCode](https://opencode.ai) skill. Install it by copying the skill directory to your OpenCode skills folder:

```bash
# Clone the repository
git clone https://github.com/your-username/fable-mode.git

# Copy to OpenCode skills directory
cp -r fable-mode ~/.config/opencode/skills/fable-mode
```

Or install manually:
1. Download or clone this repository
2. Copy the `fable-mode/` directory to `~/.config/opencode/skills/`
3. Restart OpenCode

### Requirements

- OpenCode or Claude Code runtime
- Bash access (for running verification checks)
- Sub-agent support (for `full` mode parallel delegation — optional)

---

## Usage

### Basic Mode (Default)

For multi-file tasks with sequential steps:

```
fable mode on — add input validation to the registration form
```

The agent will:
1. Plan the work in stages
2. Execute each stage sequentially
3. Run verification checks after each stage
4. Produce a final proof and self-review

### Full Mode (Parallel)

For large tasks where stages can run concurrently:

```
fable mode full — migrate our API from v1 to v2, touching 12 endpoints
```

Independent stages are dispatched in parallel using sub-agents.

### Lite Mode (Lightweight)

For moderate tasks with 2-4 steps:

```
fable mode lite — refactor the utility functions in src/utils
```

Simplified pipeline: plan → execute → verify → deliver.

### Trigger Keywords

Fable mode activates when the user says:
- "fable mode" / "fable mode on"
- "Hero's Guild"
- "staged execution"
- "stage plan"
- "proof of deeds"
- "disciplined execution"

### When It Does NOT Trigger

| Scenario | Why |
|----------|-----|
| Single-file edit | Trivially verifiable in one step |
| Quick question | No stages to decompose |
| User declines | Respect the refusal |

---

## How It Works

### The 4 Rites

```
User: "fable mode on — [task]"
     |
=== Rite the First: The Cartographer's Map ===
     |-> Decompose into stages with verification checks
     |-> User accepts plan
     |
=== Rite the Second: The Summoning of Allies ===
     |-> Execute stages (sequential or parallel)
     |
=== Rite the Third: The Proof of Deeds ===
     |-> Run verification check after each stage
     |-> Final Proof: does the whole hold together?
     |
=== Rite the Fourth: The Mirror of the Guild ===
     |-> Self-review: hidden complexity? assumptions? weakened checks?
     |
=== DELIVERY ===
     |-> Deliver with evidence, not confidence
```

### The Gate

Before fable mode activates, two conditions must be met:

1. **The task cannot be done in one confident step** — If it can, fable mode is overhead
2. **Discipline reduces failure risk** — Not just pageantry

### Verification Checks

Every stage must have a verification check that is:
- **Concrete** — A specific command (`pytest`, `npx tsc --noEmit`, `npm run lint`)
- **Machine-verifiable** — Runs as a tool call, produces pass/fail output
- **Honest** — Can genuinely fail. "Looks good" cannot fail honestly.

**Good check:**
```
pytest --tb=short → exits 0 with all tests passing
```

**Bad check:**
```
"Does the API response look correct?" → subjective, no evidence
```

---

## Agent Team

| Agent | Role | Active During |
|-------|------|---------------|
| **Cartographer** | Decomposes tasks into stages, defines verification checks | Rite the First |
| **Blacksmith** | Executes stages, runs checks, handles failures | Rite the Second + Third |
| **Sage** | Final Proof of Deeds — validates integration and requirements | Rite the Third (final) |
| **Mirror** | Self-review — catches hidden complexity and process flaws | Rite the Fourth |

Each agent has a defined scope and cannot cross into another agent's territory.

---

## Project Structure

```
fable-mode/
├── SKILL.md                              # Core skill definition (326 lines)
├── README.md                             # This file
│
├── agents/
│   ├── cartographer_agent.md             # Planning agent — stage decomposition
│   ├── blacksmith_agent.md               # Execution agent — run stages & checks
│   ├── sage_agent.md                     # Verification agent — Final Proof
│   └── mirror_agent.md                  # Self-review agent — skeptical check
│
├── references/
│   ├── check_types_by_domain.md          # Verification patterns (code/research/data)
│   ├── gate_protocol.md                  # When to enter/exit fable mode
│   ├── failure_paths.md                  # 12 failure scenarios with recovery
│   ├── parallel_delegation_guide.md      # How to dispatch parallel stages
│   ├── verification_examples.md          # 15+ concrete check examples
│   └── changelog.md                      # Version history
│
├── templates/
│   ├── stage_plan_template.md            # Fillable stage plan format
│   ├── final_proof_template.md           # Final proof checklist
│   └── delivery_summary_template.md      # Delivery summary format
│
└── examples/
    ├── multi_file_refactor.md            # Code refactor example (5 stages)
    ├── api_migration.md                  # API migration with parallel delegation
    └── research_project.md               # Research/writing project example
```

**17 files · 2,652 lines · 6 categories**

---

## Hard Limits

These rules always apply — no exceptions:

1. Never execute a stage without its check written in the plan
2. Never mark a stage complete without running its check
3. Never proceed past a failed check — fix first
4. Never deliver until the Final Proof passes
5. If a fix changes prior output, re-run that stage's check

---

## Failure Handling

| # | Failure | Strategy |
|---|---------|----------|
| F1 | Stage check fails | Diagnose, fix, re-run |
| F2 | Check fails 3x consecutively | STOP — escalate to user |
| F3 | Final Proof fails | Re-run failing stage from forge |
| F4 | Circular dependencies | Redesign stage plan |
| F5 | Parallel stage conflicts | Serialize conflicting stages |
| F6 | Scope creep | Freeze scope, finish current plan |
| F7 | Tool unavailable | Mark UNVERIFIED, flag explicitly |
| F8 | Flaw found after delivery | Recall, fix, re-deliver |

See [`references/failure_paths.md`](references/failure_paths.md) for the complete failure path map.

---

## Examples

| Example | Scenario | Stages | Mode |
|---------|----------|--------|------|
| [Multi-File Refactor](examples/multi_file_refactor.md) | Extract shared auth logic across 5 files | 5 | basic |
| [API Migration](examples/api_migration.md) | Migrate 12 endpoints from v1 to v2 | 8 | full |
| [Research Project](examples/research_project.md) | Research report with lit review + analysis | 6 | basic |

Each example shows the complete fable mode workflow: gate evaluation → stage plan → execution → proof → mirror → delivery.

---

## When to Use Fable Mode

| Task Complexity | Recommended Approach |
|----------------|---------------------|
| 1 file, 1 change | Normal execution |
| 2-4 files, moderate risk | `fable mode lite` |
| 5+ files, sequential steps | `fable mode` (basic) |
| 8+ files, parallel possible | `fable mode full` |
| Security-critical, high-risk | `fable mode full` + extra review |

---

## Contributing

Contributions are welcome. Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow the existing file structure and naming conventions
- Agent files must include: Role Definition, Phase Boundary, Core Principles, Process
- Reference files must include concrete examples and tables
- Templates must be fillable with clear instructions
- Examples must show the complete fable mode workflow

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

Inspired by the Hero's Guild of Bowerstone.
Because any fool can swing a sword — but a Hero plans the battle first.
