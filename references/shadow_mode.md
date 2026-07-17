# Shadow Mode — Non-Blocking Observation Mode

## Overview

Shadow mode lets fable mode operate in observation-only mode. In shadow mode, all fable mode protocols are followed — gates, stages, checks, proofs — but **no enforcement is applied**. The agent executes normally while fable mode logs what it *would* have done, what it *would* have blocked, and where it *would* have raised flags.

Shadow mode is the safe adoption path. Run with `FABLE_SHADOW=1` for a week, review the shadow logs, then turn enforcement on with confidence.

---

## When to Use Shadow Mode

| Scenario | Why Shadow Mode |
|----------|-----------------|
| **First adoption** | Teams new to fable mode can observe its decisions without disruption |
| **Tuning stage plans** | Calibrate check strictness before enforcing |
| **Assessing risk** | See how often scope drift occurs before blocking it |
| **Debugging false positives** | Identify checks that fire incorrectly before they block work |
| **Compliance audit** | Capture what fable mode would enforce without slowing delivery |

---

## Enabling Shadow Mode

Set the `FABLE_SHADOW` environment variable before invoking fable mode:

```bash
export FABLE_SHADOW=1
fable mode on — refactor the auth module across 5 files
```

Alternatively, include it in the invocation:

```bash
FABLE_SHADOW=1 fable mode on — [task]
```

### Behavior When Enabled

| Protocol | Shadow Behavior |
|----------|-----------------|
| Gate check | Logs whether the task passes the gate, but executes regardless |
| Stage plan | Plans are produced and logged, but execution proceeds without blocking |
| Verification checks | Checks run and results are logged, but failures do not stop execution |
| Scope guard | Drift is logged with warnings, but files are not reverted |
| Final Proof | Proof runs and results are logged, but delivery is not blocked |
| Mirror review | Findings are logged, but delivery proceeds even if flaws are found |

---

## Shadow Log Format

Shadow logs are written to `.fable/shadow/`:

```
.fable/shadow/
├── run-2026-07-17T10-30-00/     # Per-run directory
│   ├── shadow.audit.jsonl        # All shadow decisions as JSONL
│   └── summary.md                # Human-readable summary
└── latest -> run-2026-07-17T10-30-00/  # Symlink to latest run
```

### Shadow Audit Entry

```json
{
  "entry_id": "shd-001",
  "timestamp": "2026-07-17T10:30:00Z",
  "run_id": "run-auth-refactor-7a92",
  "category": "gate",
  "decision": "WOULD_PASS",
  "detail": "Task meets complexity threshold (5 files, 3 stages) and risk threshold (auth failure is breaking)",
  "would_block": false,
  "actual_outcome": "ALLOWED"
}
```

### Decision Categories

| `category` | Possible `decision` Values | `would_block` |
|------------|---------------------------|---------------|
| `gate` | `WOULD_PASS`, `WOULD_FAIL` | `false` |
| `check` | `WOULD_PASS`, `WOULD_FAIL` | `true` if check would fail |
| `scope` | `IN_SCOPE`, `DRIFT_DETECTED` | `true` if drift detected |
| `proof` | `WOULD_PASS`, `WOULD_FAIL` | `true` if proof would fail |
| `mirror` | `CLEAN`, `FLAW_DETECTED` | `true` if flaw found |
| `bounce` | `WOULD_BOUNCE`, `WOULD_CONTINUE` | `true` if would bounce |

---

## Reviewing Shadow Logs

### Quick Summary

```bash
cat .fable/shadow/latest/summary.md
```

Example output:

```markdown
# Shadow Run Summary

**Run**: run-auth-refactor-7a92
**Date**: 2026-07-17T10:30:00

## Gate
- **Decision**: WOULD_PASS (complexity: 5 files, risk: high)

## Checks (4 stages)
| Stage | Check | Decision | Would Block |
|-------|-------|----------|-------------|
| Stage 1: Refactor auth | npx tsc --noEmit | PASS | No |
| Stage 2: Update tests | pytest tests/auth/ | PASS | No |
| Stage 3: Update API | npx tsc --noEmit | FAIL | Yes |
| Stage 4: Integration | npm test | PASS | No |

## Scope Drift
- Stage 3 modified src/utils/helpers.ts (not in planned files)

## Mirror
- Clean: no flaws detected

## Would Have Blocked: 1 action
- Stage 3: type check failed (exit code 2, 3 errors)
- Scope drift: Stage 3 touched unplanned file

## Recommendation
- Turn on full enforcement after fixing Stage 3's type errors
- Add src/utils/helpers.ts to Stage 3's planned files
```

### Aggregated Report

```bash
# Count all shadow decisions by category
grep '"category"' .fable/shadow/*/shadow.audit.jsonl | sort | uniq -c

# Find all decisions that would have blocked
grep '"would_block": true' .fable/shadow/*/shadow.audit.jsonl
```

---

## Shadow-to-Enforcement Transition

### Confidence Threshold

Transition from shadow to enforcement when:

| Metric | Threshold | Meaning |
|--------|-----------|---------|
| False positive rate | < 5% | Checks reliably indicate real failures |
| Scope drift rate | < 10% | Planned files are correctly declared |
| Gate accuracy | > 95% | Tasks are correctly routed to fable or normal mode |
| Check integrity | 100% | All checks are machine-verifiable and can fail |

### Transition Process

1. **Review** shadow logs from at least 5 runs or 1 week.
2. **Fix** false positives (adjust checks that block incorrectly).
3. **Update** stage plans that have incorrect scope declarations.
4. **Announce** the transition to the team.
5. **Set `FABLE_SHADOW=0`** or unset the variable.
6. **Monitor** enforcement for the first week. Keep shadow logs as a fallback.

### Rollback

If enforcement causes problems:
1. Set `export FABLE_SHADOW=1` to return to observation mode.
2. Review the enforcement logs to identify the issue.
3. Fix the plan, checks, or scope declarations.
4. Retry enforcement.

---

## Fable Mode Integration

### In SKILL.md

Add to the Quick Start section:

```
**Shadow mode (safe adoption):**
FABLE_SHADOW=1 fable mode on — [task]
```

### In Cartographer

When shadow mode is active, the Cartographer logs but does not enforce:

```
Shadow mode active: producing stage plan for observation only.
Checks will run but failures will not block execution.
```

### In Blacksmith

When shadow mode is active, the Blacksmith:

1. Runs all verification checks and logs results.
2. Logs scope drift but does not revert files.
3. Records audit entries for all actions.
4. Continues execution regardless of check failures.
5. Documents the shadow status in each stage summary.

### In Mirror

When shadow mode is active:

1. The Mirror logs all findings as usual.
2. Delivery proceeds even if flaws are found.
3. The summary includes a recommendation: enforce or continue shadow.
