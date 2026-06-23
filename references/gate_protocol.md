# Gate Protocol — When to Enter and Exit Fable Mode

## Overview

The gate is the first decision point: should this task use fable mode at all? Getting this wrong wastes time (fable mode on trivial tasks) or risks failure (no fable mode on complex tasks).

---

## Gate Conditions

Before invoking fable mode, satisfy **both** conditions:

### Condition 1: Complexity Threshold

The task **cannot** be completed in one confident, verifiable step.

**How to evaluate:**

| Factor | One Step (No Fable) | Multiple Steps (Fable) |
|--------|---------------------|------------------------|
| Files touched | 1 file | 2+ files |
| Logical actions | 1 action | 2+ distinct actions |
| Verification | 1 check proves correctness | Multiple checks needed |
| Confidence | High (you've done this before) | Medium or low (novel, complex, risky) |

**Examples:**
- "Fix the typo in README.md" → One step. No fable.
- "Refactor the auth module across 5 files" → Multiple steps. Fable.
- "Add a console.log for debugging" → One step. No fable.
- "Migrate API from v1 to v2, touching 12 endpoints" → Many steps. Fable.

### Condition 2: Risk Reduction Value

A disciplined approach **measurably reduces** the chance of failure — not merely adds pageantry.

**How to evaluate:**

| Risk Factor | Low Risk (No Fable) | High Risk (Fable) |
|-------------|---------------------|-------------------|
| Reversibility | Easy to undo | Hard to undo |
| Impact of failure | Annoying | Breaking, data-losing, or costly |
| Dependencies | Self-contained | Affects other systems |
| Complexity | Straightforward logic | Interconnected changes |
| Testing difficulty | Trivial to verify | Requires multiple test scenarios |

**Examples:**
- "Update a constant value" → Low risk, easy to undo. No fable.
- "Refactor database schema + update all queries" → High risk, hard to undo. Fable.
- "Change button text" → Low risk. No fable.
- "Restructure authentication + add MFA + update middleware" → High risk, many dependencies. Fable.

---

## Gate Decision Tree

```
Task received
    |
    +-- Can it be done in one confident, verifiable step?
    |   +-- YES --> Do NOT use fable mode. Execute normally.
    |   +-- NO -->
    |           |
    |           +-- Would disciplined execution reduce failure risk?
    |               +-- YES --> USE fable mode. Choose mode (basic/full/lite).
    |               +-- NO --> Do NOT use fable mode. Execute normally.
```

---

## Mode Selection After Gate

Once the gate opens, select the appropriate mode:

| Criteria | basic | full | lite |
|----------|-------|------|------|
| Number of stages | 3-7 | 8+ | 2-4 |
| Parallel runtime available | No | Yes | No |
| Stage independence | Low (sequential) | High (parallel possible) | Medium |
| User time available | Moderate | May need check-ins | Minimal |

---

## Exiting Fable Mode

Fable mode can be exited early in these situations:

### 1. Gate Fails (Before Planning)
If the Cartographer determines the task does not meet the complexity threshold, exit immediately. State:
> "This task does not require staged execution. Proceeding normally."

### 2. User Declines (During Planning)
If the user declines the stage plan or asks to exit fable mode, sheathe the scroll. State:
> "Exiting fable mode. Proceeding with normal execution."

### 3. Task Completes Early (During Execution)
If all remaining stages are trivially verifiable and the risk of failure is negligible, the Cartographer may propose early exit. This requires:
- All completed stages have passed checks
- Remaining stages are trivially simple
- User confirms early exit

### 4. Unrecoverable Failure (During Execution)
If a stage fails 3 times consecutively and cannot be fixed, STOP fable mode. Escalate to user with:
- What was attempted
- What failed (with error output)
- Why it cannot be fixed within the current approach
- Recommended alternative approaches

---

## Anti-Patterns

### Using Fable Mode for Trivial Tasks
**Symptom**: Stage plan has 1-2 stages, checks are trivially passing.
**Fix**: Exit fable mode. The overhead is not justified.

### Skipping the Gate
**Symptom**: Fable mode invoked without evaluating complexity or risk.
**Fix**: Always run the gate evaluation before planning.

### Using Fable Mode as a Crutch
**Symptom**: Every task gets fable mode "just in case."
**Fix**: Apply the gate honestly. Fable mode is for complex, risky tasks — not everything.

### Ignoring the Gate Exit
**Symptom**: Cartographer determines the task is simple, but execution continues in fable mode anyway.
**Fix**: Respect the gate. Exit when the conditions are not met.
