# Parallel Delegation Guide — Dispatching Independent Stages

## Overview

In full mode, the Blacksmith can dispatch independent stages concurrently using sub-agents or concurrent tool calls. This guide explains how to identify independent stages, dispatch them, and merge results.

---

## When to Use Parallel Delegation

Parallel delegation is available **only in full mode** and only when the runtime supports true parallel execution (sub-agents, concurrent tool calls, multi-threaded execution).

**Do NOT simulate parallelism.** If the runtime does not support true parallelism, execute stages sequentially and state this clearly.

---

## Identifying Independent Stages

A stage is **independent** if:
- Its `Depends on` field is `none`, OR
- All its dependencies have already passed their checks

**Example:**

| Stage | Dependencies | Status | Can Run in Parallel? |
|-------|-------------|--------|---------------------|
| Stage 1 | none | Not started | YES |
| Stage 2 | none | Not started | YES |
| Stage 3 | Stage 1 | Waiting on Stage 1 | NO (wait for Stage 1) |
| Stage 4 | Stage 1, Stage 2 | Waiting on both | NO (wait for Stage 1 and 2) |
| Stage 5 | Stage 3 | Waiting on Stage 3 | NO (wait for Stage 3) |

In this example, Stage 1 and Stage 2 can run in parallel. Stage 3 can start after Stage 1 completes. Stage 4 can start after both Stage 1 and Stage 2 complete.

---

## Dispatch Protocol

### Step 1: Identify Ready Stages

At any point, identify all stages whose dependencies are satisfied and that have not yet been executed.

### Step 2: Check for Resource Conflicts

Before dispatching in parallel, verify that the ready stages do not modify the same files or resources. If they do, serialize them instead.

**Conflict check:**
- Stage A modifies `src/auth/login.ts`
- Stage B modifies `src/auth/login.ts`
- → CONFLICT. Serialize: run Stage A first, then Stage B (with awareness of A's output).

### Step 3: Dispatch

For each independent stage:
1. Create a sub-agent invocation (or concurrent tool call).
2. Pass the stage's goal, expected output, and verification check.
3. Pass any input data from dependency stages (if dependencies were just completed).
4. Each sub-agent executes independently.

### Step 4: Collect Results

Wait for all parallel stages to complete. For each:
- Record the check result (PASS/FAIL/UNVERIFIED)
- Record the output
- Record any evidence

### Step 5: Handle Failures

If any parallel stage fails:
- Do NOT proceed to dependent stages.
- Handle the failure per the failure paths protocol.
- After fix, re-dispatch the failed stage.
- Re-check any stages that depended on the failed stage's output.

---

## Dispatch Patterns

### Pattern 1: Independent File Edits

**Scenario**: Edit 5 unrelated files, each with its own check.

```
Stage 1: Edit src/utils/helpers.ts → Check: npx tsc --noEmit
Stage 2: Edit src/utils/format.ts → Check: npx tsc --noEmit
Stage 3: Edit src/components/Button.tsx → Check: npx tsc --noEmit
Stage 4: Edit src/components/Input.tsx → Check: npx tsc --noEmit
Stage 5: Edit src/styles/main.css → Check: stylelint passes

All stages are independent → dispatch all 5 in parallel.
```

### Pattern 2: Mixed Independent and Dependent

**Scenario**: Refactor shared module, then update consumers.

```
Stage 1: Refactor src/shared/auth.ts → Check: pytest tests/auth.test.py
Stage 2: Refactor src/shared/permissions.ts → Check: pytest tests/permissions.test.py
Stage 3: Update src/api/users.ts (depends on Stage 1) → Check: npx tsc --noEmit
Stage 4: Update src/api/admin.ts (depends on Stage 1, Stage 2) → Check: npx tsc --noEmit

Parallel batch 1: Stage 1 + Stage 2 (independent)
Parallel batch 2: Stage 3 (after Stage 1 completes)
Parallel batch 3: Stage 4 (after Stage 1 and Stage 2 complete)
```

### Pattern 3: Test Suites in Parallel

**Scenario**: Run multiple test suites concurrently.

```
Stage 1: Run unit tests → Check: pytest tests/unit/ exits 0
Stage 2: Run integration tests → Check: pytest tests/integration/ exits 0
Stage 3: Run e2e tests → Check: pytest tests/e2e/ exits 0

All stages are independent → dispatch all 3 in parallel.
```

---

## Merging Parallel Results

After all parallel stages complete, merge their results into a single summary:

```
# Parallel Execution Summary

## Batch 1 (3 parallel stages)
| Stage | Status | Evidence |
|-------|--------|----------|
| Stage 1 | PASS | pytest: 23 passed |
| Stage 2 | PASS | pytest: 15 passed |
| Stage 3 | PASS | stylelint: 0 warnings |

## Next Steps
- Stage 4 depends on Stage 1 → Ready to dispatch
- Stage 5 depends on Stage 1 + Stage 2 → Ready to dispatch
```

---

---

## Wave-Based Execution (DAG Pattern)

For plans with multiple dependency layers, use wave-based execution. A wave is a batch of stages that can run concurrently because all their dependencies are satisfied.

### Wave Protocol

```
Wave 1: [Stages with Depends on: none]
Wave 2: [Stages whose dependencies were in Wave 1]
Wave 3: [Stages whose dependencies were in Wave 2]
...
```

### Example: 7-Stage Plan in 3 Waves

```
Stage 1: Refactor shared types     (Depends on: none)
Stage 2: Refactor auth module      (Depends on: none)
Stage 3: Refactor API client       (Depends on: none)
---
Stage 4: Update user endpoints     (Depends on: Stage 1, Stage 2)
Stage 5: Update admin endpoints    (Depends on: Stage 1, Stage 2)
---
Stage 6: Integration tests         (Depends on: Stage 3, Stage 4, Stage 5)
Stage 7: E2E tests                 (Depends on: Stage 3, Stage 4, Stage 5)
```

Execution shape:

```
Wave 1: Stage 1 + Stage 2 + Stage 3 (3 parallel)
Wave 2: Stage 4 + Stage 5 (2 parallel, after Wave 1 completes)
Wave 3: Stage 6 + Stage 7 (2 parallel, after Wave 2 completes)
```

### Wave Execution Rules

1. All stages in a wave must complete before the next wave starts.
2. If any stage in a wave fails, the entire wave is blocked until the failure is resolved.
3. Within a wave, resource conflict checks apply: stages modifying the same file must be serialized.
4. Wave execution produces a natural barrier for checkpoints and audit entries.

### Resource Conflict Matrix

Before dispatching a wave, construct a conflict matrix:

```
         | Stage 1 | Stage 2 | Stage 3
---------|---------|---------|---------
Stage 1  |    —    |  CONFLICT (same file) |  OK
Stage 2  | CONFLICT|    —     |  OK
Stage 3  |   OK    |   OK    |  —
```

- **CONFLICT**: Both stages modify the same file → serialize them within the wave.
- **OK**: No shared files → safe to run in parallel.

Conflict detection uses the `Planned Files` field from the stage plan.

### Fan-Out / Fan-In Pattern

**Fan-Out**: One stage produces output consumed by N downstream stages.

```
Stage 1: Define shared types (output: src/types.ts)
  → Stage 2: Update auth (depends on Stage 1)
  → Stage 3: Update API (depends on Stage 1)
  → Stage 4: Update UI (depends on Stage 1)
```

**Fan-In**: N stages produce output consumed by one downstream stage.

```
Stage 1: Refactor middleware    \
Stage 2: Refactor routes         > Stage 4: Integration tests
Stage 3: Refactor validators   /
```

### Wave Boundary Checkpoints

After each wave completes, save a checkpoint:

```markdown
# Wave Checkpoint: [Task Name]
**Wave**: [N of Total]
**Completed Stages**:
- Stage 1: PASS (evidence: ...)
- Stage 2: PASS (evidence: ...)
- Stage 3: PASS (evidence: ...)
**Next Wave**: Stage 4, Stage 5 (dependencies satisfied)
**Audit Trail**: chain.audit.jsonl entries aud-001 to aud-008 (hash chain intact)
```

---

## Anti-Patterns

### Simulating Parallelism
**Wrong**: Describing parallel execution but actually running sequentially.
**Right**: Either truly dispatch concurrently or state clearly that sequential execution is used.

### Dispatching Dependent Stages in Parallel
**Wrong**: Dispatching Stage 3 (which depends on Stage 1) at the same time as Stage 1.
**Right**: Wait for Stage 1 to complete before dispatching Stage 3.

### Ignoring Resource Conflicts
**Wrong**: Dispatching two stages that both modify the same file.
**Right**: Check for conflicts and serialize if needed.

### Not Waiting for All Results
**Wrong**: Proceeding to dependent stages before all parallel stages complete.
**Right**: Wait for all parallel stages to complete and collect all results before proceeding.

### Failing to Record Wave Boundaries
**Wrong**: Not checkpointing between waves, losing cross-wave context.
**Right**: Save a checkpoint and audit entry after each wave completes.
