---
name: fable-mode
description: >
  Enforces staged execution discipline on large tasks: first a gate (trivial tasks refused),
  then a written stage plan, parallel delegation where the runtime allows,
  a failable verification check after each stage,
  a final proof-of-deeds summary, and a skeptical self-review before delivery.
  Trigger when the user explicitly asks ("fable mode" or "fable mode on") or requests Hero's Guild discipline.
  Do NOT trigger on single-step tasks, quick questions, brainstorming sessions, or when the user explicitly declines.
metadata:
  version: "2.1.0"
  last_updated: "2026-06-24"
  status: active
  data_access_level: raw
  task_type: multi-step
  related_skills: []
---

# Fable Mode — Staged Execution Discipline for AI Agents

A task execution framework that refuses to let agents work beneath their own standard. Decomposes complex work into stages with machine-verifiable checks, enforces proof before delivery, and demands skeptical self-review.

**v1.0** — Initial published release:
- **3 Modes** — `basic` (standard staged execution), `full` (with parallel delegation), `lite` (lightweight for smaller tasks)
- **4 Agents** — Cartographer (planning), Blacksmith (execution), Sage (verification), Mirror (self-review)
- **4 Rites** — Cartographer's Map, Summoning of Allies, Proof of Deeds, Mirror of the Guild
- **Templates** — Stage plan, final proof, delivery summary
- **Failure Paths** — What to do when checks fail, delegation fails, scope creeps
- **Domain-Specific Checks** — Patterns for code, research, data, and multi-step tasks

**v2.0** — Fable 5 enhanced release:
- **7 Agents** — Added Researcher (research tasks), DataAnalyzer (data tasks), Deployer (deployment tasks)
- **Vision Verification** — Screenshot comparison, design fidelity, accessibility checks
- **Self-Verification** — Write-your-own-tests, reasoning re-derivation, output-against-goal
- **Dynamic Re-planning** — Revise plans when evidence contradicts the hypothesis
- **Long-Session Support** — Checkpoint system, context compaction, session resume
- **Memory Integration** — Cross-session memory for verified facts and user preferences

**v2.1** — Fable 5 deep integration:
- **Effort Control** — Different thinking depth for different stages (high/xhigh for planning, medium for execution)
- **Fresh-Context Verification** — Separate verifier subagent for final review
- **Progress Grounding** — Every progress claim must cite tool evidence
- **Brevity Instructions** — Prevent over-elaboration, lead with outcomes
- **Fallback Handling** — Graceful degradation when Fable 5 refuses or fails
- **Structured Memory** — Lessons learned, corrections, confirmed approaches

---

## Quick Start

**Minimal command:**
```
fable mode on — refactor the authentication module across 5 files
```

**With explicit mode:**
```
fable mode full — migrate our API from v1 to v2, touching 12 endpoints
```

**Lite mode for moderate tasks:**
```
fable mode lite — add input validation to the registration form
```

**Execution:**
1. Gate check — Is this task complex enough to warrant fable mode?
2. Cartographer's Map — Decompose into stages with verification checks
3. Summoning of Allies — Dispatch independent stages in parallel (full mode only)
4. Proof of Deeds — Run verification checks after each stage
5. Final Proof — Validate the whole holds together
6. Mirror of the Guild — Skeptical self-review before delivery

---

## Trigger Conditions

### Trigger Keywords

**English**: fable mode, fable mode on, Hero's Guild, staged execution, stage plan, proof of deeds, disciplined execution, rigorous execution, fable discipline

### Does NOT Trigger

| Scenario | Why | Use Instead |
|----------|-----|-------------|
| Single-file edit | Trivially verifiable in one step | Normal execution |
| Quick question or brainstorm | No stages to decompose | Normal execution |
| User explicitly declines | Respect the refusal | Normal execution |
| Task spans one file, one change | Gate condition fails — not complex enough | Normal execution |

### Gate Conditions

Before invoking Fable Mode, satisfy **both**:

1. **The quest cannot be completed in one confident, verifiable step.** If you can do it in one shot with high confidence, fable mode is overhead, not discipline.
2. **A disciplined approach would measurably reduce the chance of failure** — not merely add pageantry.

*If either condition fails, answer normally. Do not invoke the rites.*

> **Asking the user:** If the task is colossal and you suspect the user would benefit, you may ask **once** — but if they decline, sheathe the scroll immediately and proceed plainly.

---

## Mode Selection Guide

```
User Input
    |
    +-- Is the task trivially simple (1 file, 1 change)?
    |   +-- Yes --> Do NOT use fable mode. Execute normally.
    |   +-- No --> Is the task large (5+ files, 3+ logical steps)?
    |               +-- Yes --> Does the runtime support parallel delegation?
    |               |           +-- Yes --> full mode
    |               |           +-- No --> basic mode
    |               +-- No (moderate, 2-4 steps) --> lite mode
```

| Mode | When to Use | What You Get |
|------|-------------|--------------|
| `basic` | Multi-file, sequential steps, no parallel runtime | Full 4-rite pipeline, sequential execution |
| `full` | Large tasks, parallel runtime available | Full 4-rite pipeline + parallel delegation of independent stages |
| `lite` | Moderate tasks, 2-4 steps | Simplified map + proof + mirror (skip formal agent dispatch) |

---

## Agent Team (7 Agents)

| # | Agent | Role | When Active |
|---|-------|------|-------------|
| 1 | `cartographer_agent` | Decomposes the task into stages, writes the stage plan, defines verification checks | Rite the First |
| 2 | `blacksmith_agent` | Executes each stage, runs verification checks, handles failures and retries | Rite the Second + Third |
| 3 | `sage_agent` | Conducts the Final Proof of Deeds, validates cross-stage integration | Rite the Third (final) |
| 4 | `mirror_agent` | Performs self-review, identifies hidden complexity, checks assumptions | Rite the Fourth |
| 5 | `researcher_agent` | Handles literature search, source verification, synthesis | When Blacksmith delegates research stages |
| 6 | `data_analyzer_agent` | Handles schema validation, data integrity, edge cases | When Blacksmith delegates data stages |
| 7 | `deployer_agent` | Handles deployment checks, rollback, environment verification | When Blacksmith delegates deployment stages |

---

## Orchestration Workflow (4 Rites)

```
User: "fable mode on — [task]"
     |
=== Rite the First: The Cartographer's Map ===
     |
     |-> [cartographer_agent] -> Stage Plan
     |   - Decompose task into discrete stages
     |   - Define expected output per stage
     |   - Define failable verification check per stage
     |   - Identify dependencies between stages
     |   - Define Final Proof check
     |
     +-> Gate check: Does this plan meet the complexity threshold?
         - If NO → Exit fable mode, execute normally
         - If YES → Present plan to user for acceptance
         |
     ** User acceptance before proceeding **
     |
=== Rite the Second: The Summoning of Allies ===
     |
     |-> [blacksmith_agent] -> Stage Execution
     |   - If full mode: dispatch independent stages in parallel
     |   - If basic/lite mode: execute stages sequentially
     |   - Each stage is a separate deed — never blur two into one
     |
=== Rite the Third: The Proof of Deeds ===
     |
     |-> [blacksmith_agent] -> Per-Stage Verification
     |   - Run the verification check defined in the map
     |   - Check must be a FACT, not a feeling
     |   - If check fails → return to forge, fix, re-run
     |   - If fix changes prior stage output → re-run that stage's check
     |
     +-> [sage_agent] -> Final Proof of Deeds
         - Validate all stages integrate correctly
         - Verify original requirements are demonstrably met
         - Check for regressions, loose ends, incomplete work
         - If Final Proof fails → identify failing stage, re-run from forge
         |
=== Rite the Fourth: The Mirror of the Guild ===
     |
     +-> [mirror_agent] -> Self-Review
         - Hidden complexity skipped?
         - Assumptions that may not hold?
         - Brittle, insecure, or undocumented patterns?
         - Any check weakened after seeing output?
         - If Mirror shows flaw → fix BEFORE delivery
         |
=== DELIVERY ===
     |
     +-> Deliver final output with summary
```

---

## Hard Limits — Always Apply, Never Override

1. Never begin executing a stage without its check written in the map.
2. Never mark a stage complete without running its check as a verifiable fact.
3. Never proceed past a failed check — return to the forge first.
4. Never deliver output until the Final Proof passes.
5. If a fix changes a prior stage's output, re-run that stage's check before continuing.

---

## Rite the First: The Cartographer's Map

*A plan written in ink, not in hope.*

See `agents/cartographer_agent.md` for full agent definition.
See `templates/stage_plan_template.md` for the output format.

1. Decompose the quest into discrete **stages**.
2. For each stage, name:
   - The goal.
   - The expected concrete output (file, data structure, proof).
   - **A failable verification check** — a question whose answer can be "no".
   - **Dependencies** — which other stages must be complete before this one can begin (write `none` if independent).
3. **Also define the Final Proof check now**, before drawing the map — what would prove the whole holds together?
4. Present the map in the standard format (see template).
5. Do **not** proceed until the map is accepted (if the user is available).
   *If no user is present (agentic context), validate the map yourself: check that every stage has a concrete output and a failable check, and that the stages together fully cover the original requirements with no gaps.*

### Verification Checks That Earn Guild Approval

| Path | Domain | Example Check | Why It Works |
|------|--------|---------------|--------------|
| Blacksmith | Code | `pytest --tb=short` exits 0 with all tests passing | Can fail, produces verifiable evidence |
| Blacksmith | Code | `npx tsc --noEmit` exits 0 | Type errors are concrete failures |
| Blacksmith | Code | `npm run lint` reports no new warnings | Linter output is machine-readable |
| Sage | Research | Every factual claim traced to a source, no contradictory statements | Verifiable against sources |
| Sage | Data | Row counts match, schema constraints hold, edge cases tripped | Data integrity is checkable |
| Sage | Multi-step | Each stage's output consumed by the next without manual glue | Integration is testable |

### Rejected Checks (Cannot Fail Honestly)

- "Does it look right?" — subjective, produces no evidence
- "Is the API response correct?" — vague, no pass/fail criterion
- "Should work now" — reasoning-based, not tool-verified
- "Looks good to me" — feelings, not facts

---

## Rite the Second: The Summoning of Allies

*Many hands make light work — but only when the realm permits it.*

See `agents/blacksmith_agent.md` for full agent definition.
See `references/parallel_delegation_guide.md` for delegation patterns.

- **Full mode only:** If the runtime supports true parallel delegation (sub-agents, concurrent tools), dispatch the independent stages of the map.
  *Independent stages are those whose **Depends on** field is `none` or whose dependencies are already satisfied.*
- **Basic/lite mode:** Work through the stages one by one. Do not simulate parallelism.
- Whether parallel or sequential, treat each stage as a separate deed — never blur two into one.

---

## Rite the Third: The Proof of Deeds

*A Hero does not hope the arrow hit the mark. A Hero walks to the target and looks.*

See `agents/blacksmith_agent.md` and `agents/sage_agent.md` for full agent definitions.
See `references/check_types_by_domain.md` for domain-specific check patterns.
See `references/verification_examples.md` for concrete examples.

**After every stage**, before marking it complete, perform the failable verification check written in the map.

- The check must be a **fact**, not a feeling.
  *In agentic runtimes (OpenCode, Claude Code, any bash-capable environment): run the check as a tool call — bash command, test runner, file read, linter invocation. A reasoning-based conclusion ("it should work") is not a check. If no tool can verify the claim, mark the stage output as **unverified** and flag it explicitly before continuing.*
- If the check fails, the stage is **not** complete. Return to the forge.
  **Crucially:** If your fix changes the output of a prior stage, re-run that stage's check before continuing. Do not let a repaired roof collapse the walls.
- Only when all individual stages have passed their checks may you conduct the **Final Proof of Deeds** — a single, high-level verification that the whole quest holds together (all pieces connected, no regressions, no loose ends).
  **If the Final Proof fails:** identify which stage's output — or the interface between stages — caused the failure, re-run that stage from the forge, and then re-run the Final Proof. Do not deliver until the Final Proof passes.

---

## Rite the Fourth: The Mirror of the Guild

*Now stop. Put down the sword. Look at what you have built — as your own sternest critic.*

See `agents/mirror_agent.md` for full agent definition.

- Review the **process**, not just the result.
- Work through this self-check before delivery:
  - Hidden complexity skipped? *(If yes → identify it; fix it or flag it explicitly in the delivery summary.)*
  - Assumptions that may not hold in the user's environment? *(If yes → state them plainly in the delivery summary.)*
  - Brittle, insecure, or undocumented patterns used? *(If yes → replace or document before delivering.)*
  - Any check weakened or softened after seeing the output? *(If yes — this is a process flaw, not a result flaw → identify the check, re-design it honestly, re-run it.)*
- If the Mirror shows a flaw, fix it **before** delivery.
  *If the fix alters any stage's output, re-run that stage's Proof check before delivering. The Mirror reveals — the Proof confirms.*
- When the Mirror is clean, deliver the final output with a brief summary of what was done and why it can be trusted.

**Distinction** — The Mirror is not the Proof.
- The Proof checks *what was produced*.
- The Mirror checks *how it was produced* and whether anything was silently swept under the rug.

---

## Failure Paths

See `references/failure_paths.md` for the complete failure path map.

| # | Failure Scenario | Severity | Handling Strategy |
|---|---------|---------|---------|
| F1 | Stage check fails repeatedly (3+ retries) | High | STOP — escalate to user with full context |
| F2 | Final Proof fails | Critical | Identify failing stage, re-run from forge |
| F3 | Dependencies create circular reference | High | Redesign stage plan, break the cycle |
| F4 | Parallel delegation produces conflicts | Medium | Serialize conflicting stages |
| F5 | Scope creep mid-execution | Medium | Freeze scope, complete current plan, plan new quest |
| F6 | User interrupts mid-stage | Low | Save progress, summarize state |
| F7 | Verification tool unavailable | Medium | Mark stage as unverified, flag explicitly |
| F8 | Mirror finds flaw after delivery | High | Recall, fix, re-run Proof, re-deliver |

---

## Effort Control (Fable 5)

Fable 5 uses an effort parameter to control thinking depth. Use different effort levels for different stages:

| Stage | Recommended Effort | Why |
|-------|-------------------|-----|
| Cartographer (planning) | `high` or `xhigh` | Planning requires deep reasoning about decomposition and dependencies |
| Blacksmith (execution) | `medium` or `high` | Execution needs good reasoning but not maximum depth |
| Sage (verification) | `high` | Verification requires careful analysis of integration |
| Mirror (self-review) | `xhigh` | Self-review benefits from deepest thinking to catch subtle issues |
| Researcher | `high` | Research requires thorough source analysis |
| DataAnalyzer | `medium` or `high` | Data validation needs good reasoning |
| Deployer | `medium` | Deployment checks are more procedural |

**When to adjust effort:**
- Increase to `xhigh` if a stage fails repeatedly and the issue is subtle
- Decrease to `medium` if a stage completes too slowly without quality loss
- Use `high` as default when uncertain

**Preventing over-elaboration at high effort:**
```
Don't add features, refactor, or introduce abstractions beyond what the task requires.
A bug fix doesn't need surrounding cleanup and a one-shot operation usually doesn't need a helper.
Don't design for hypothetical future requirements: do the simplest thing that works well.
Avoid premature abstraction and half-finished implementations.
Don't add error handling, fallbacks, or validation for scenarios that cannot happen.
Trust internal code and framework guarantees. Only validate at system boundaries.
```

---

## Brevity Instructions (Fable 5)

Claude Fable 5 can elaborate beyond what the task needs. Add these instructions to agents:

**For user-facing messages:**
```
Lead with the outcome. Your first sentence after finishing should answer "what happened"
or "what did you find": the thing the user would ask for if they said "just give me the TLDR."
Supporting detail and reasoning come after.

Being readable and being concise are different things, and readability matters more.
The way to keep output short is to be selective about what you include (drop details that
don't change what the reader would do next), not to compress the writing into fragments,
abbreviations, arrow chains like A → B → fails, or jargon.
```

**For tool-call narration:**
```
Terse shorthand is fine between tool calls (that's you thinking out loud, and brevity
there is good). Your final summary is different: it's for a reader who didn't see any of that.

If you've been working for a while without the user watching (overnight, across many
tool calls, since they last spoke), your final message is their first look at any of it.
Write it as a re-grounding, not a continuation of your working thread: the outcome first,
then the one or two things you need from them, each explained as if new.
```

---

## Progress Grounding (Fable 5)

On long autonomous runs, ground every progress claim in actual tool results:

```
Before reporting progress, audit each claim against a tool result from this session.
Only report work you can point to evidence for; if something is not yet verified, say
so explicitly. Report outcomes faithfully: if tests fail, say so with the output; if a
step was skipped, say that; when something is done and verified, state it plainly
without hedging.
```

**Enforcement in fable mode:**
- Every PASS verdict must include the tool output that proves it
- Every claim about what was done must reference a specific tool call
- "I believe it works" is not a progress claim. "pytest exited 0 with 47 tests passing" is.
- If you cannot point to evidence, mark the stage as UNVERIFIED

---

## The Guild's Final Word

This discipline does not grant new power. It only refuses to let you work beneath your own standard.
Use it when the quest demands it, and sheathe it when it does not.

---

## Long-Session Support (Fable 5)

For tasks that span multiple days or require context persistence across sessions.

### Checkpoint System

After each stage passes its check, save a checkpoint:

```markdown
# Checkpoint: [Task Name]
**Saved**: [timestamp]
**Current Stage**: [N of M]
**Completed Stages**:
- Stage 1: [name] — PASS (evidence: [key output])
- Stage 2: [name] — PASS (evidence: [key output])
**Context Summary**: [compressed summary of what's been done, decisions made, and what comes next]
**Pending**: [next stage name and goal]
**User Decisions**: [any user choices made during this session]
```

### Context Compaction

When context window fills up:
1. Summarize completed stages into a compact summary.
2. Keep: stage names, check results, key outputs, user decisions.
3. Drop: detailed execution logs, intermediate outputs.
4. Save the summary to the checkpoint file.

### Session Resume

When resuming a paused session:
1. Read the checkpoint file.
2. Reconstruct context from the summary.
3. Verify completed stages are still valid (files exist, checks still pass).
4. Resume from the next pending stage.

### Memory Integration

Use Fable 5's memory tool to persist cross-session information:

**What to remember:**
- Verified facts (e.g., "project uses PostgreSQL 15, not 14")
- User preferences (e.g., "user prefers functional style, no classes")
- Failure patterns (e.g., "this test suite is flaky, retry twice")
- Environment details (e.g., "CI runs on Ubuntu 22.04, Node 20")

**What NOT to remember:**
- Secrets or credentials
- Temporary debugging information
- Outdated assumptions that were corrected

**Memory format:**
```
Fact: [what you learned]
Source: [how you learned it — which stage, which check]
Confidence: [high/medium/low]
Last verified: [timestamp]
```

### Send-to-User Tool

Use Fable 5's `send_to_user` tool to communicate with the user during execution:

**When to use:**
- Blocked on a decision that requires user input
- Major milestone reached (e.g., "Stage 3 complete, proceeding to Stage 4")
- Error encountered that requires user decision (e.g., "Test failed — fix now or skip?")
- Deliverable ready for review

**When NOT to use:**
- Minor status updates (use progress grounding instead)
- Questions that can be answered by continuing exploration
- Requests for permission to proceed (assume autonomy unless blocked)

**Pattern:**
```
send_to_user(
  subject: "[Decision needed] API endpoint design",
  body: "Two approaches found:
    A) REST with /api/v1/users (traditional)
    B) GraphQL with /api/graphql (flexible)
    
    Recommendation: B (matches existing schema)
    
    Please confirm A or B."
)
```

**Response handling:**
- If user responds: incorporate into next stage
- If user doesn't respond: continue with best judgment, document assumption
- If user declines: adjust plan, re-run affected stages

---

## References

| Document | Purpose |
|----------|---------|
| `agents/cartographer_agent.md` | Planning agent — stage decomposition, check definition, dynamic re-planning |
| `agents/blacksmith_agent.md` | Execution agent — stage execution, verification, self-verification, re-plan triggers |
| `agents/sage_agent.md` | Verification agent — Final Proof of Deeds |
| `agents/mirror_agent.md` | Self-review agent — skeptical quality check |
| `agents/researcher_agent.md` | Research specialist — source verification, synthesis, contradiction detection |
| `agents/data_analyzer_agent.md` | Data specialist — schema validation, integrity checks, edge cases |
| `agents/deployer_agent.md` | Deployment specialist — environment checks, rollback, smoke tests |
| `references/check_types_by_domain.md` | Verification check patterns by domain |
| `references/gate_protocol.md` | Gate conditions in detail |
| `references/failure_paths.md` | Complete failure path map |
| `references/parallel_delegation_guide.md` | Parallel delegation patterns |
| `references/verification_examples.md` | 20+ concrete check examples (including vision & self-verification) |
| `references/fallback_guide.md` | Handling refusals and fallback mechanisms |
| `references/memory_system.md` | Cross-session learning and memory structure |
| `references/changelog.md` | Version history |
| `templates/stage_plan_template.md` | Fillable stage plan format |
| `templates/final_proof_template.md` | Final proof checklist |
| `templates/delivery_summary_template.md` | Delivery summary format |
| `examples/multi_file_refactor.md` | Fable mode on a code refactor |
| `examples/api_migration.md` | Fable mode on an API migration |
| `examples/research_project.md` | Fable mode on a research project |

— *Bowerstone, Hero's Guild*
