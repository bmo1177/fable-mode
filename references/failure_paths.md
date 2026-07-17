# Failure Paths — Fable Mode Failure Path Map

## Overview

This document lists all failure scenarios that may be encountered during fable mode execution, along with detection conditions, handling steps, and recovery paths. The purpose is to ensure every failure has a clear handling strategy, preventing dead ends.

---

## Failure Path Summary

| # | Failure Scenario | Severity | Handling Strategy |
|---|---------|---------|---------|
| F1 | Stage verification check fails | Medium | Diagnose, fix, re-run check |
| F2 | Stage check fails 3 consecutive times | High | STOP, escalate to user |
| F3 | Final Proof fails | Critical | Identify failing stage, re-run from forge |
| F4 | Dependencies create circular reference | High | Redesign stage plan, break cycle |
| F5 | Parallel delegation produces conflicts | Medium | Serialize conflicting stages |
| F6 | Scope creep mid-execution | Medium | Freeze scope, complete current plan |
| F7 | User interrupts mid-stage | Low | Save progress, summarize state |
| F8 | Verification tool unavailable | Medium | Mark stage UNVERIFIED, flag explicitly |
| F9 | Mirror finds flaw after delivery | High | Recall, fix, re-run Proof, re-deliver |
| F10 | Prior stage regression after fix | High | Re-run all affected stage checks |
| F11 | Cartographer plan has gaps | Medium | Return to Cartographer, revise plan |
| F12 | Agent produces non-verifiable output | Medium | Flag as UNVERIFIED, do not mark PASS |
| F13 | Bounce target chain too long | Medium | Re-plan dependencies, shorten chain |
| F14 | Scope drift detected | Medium | Revert unplanned changes or revise plan |
| F15 | Audit trail integrity check fails | Critical | Stop — possible tampering detected |

---

## Detailed Failure Paths

### F1: Stage Verification Check Fails

**Severity**: Medium

**Trigger**: The Blacksmith runs a stage's verification check and it returns non-zero exit code or error output.

**Handling Steps**:
1. Read the check output carefully. The answer is in the output.
2. Diagnose the root cause (not the symptom).
3. Fix the root cause.
4. Re-run the check.
5. If the fix changed a prior stage's output, re-run that prior stage's check.

**Recovery Paths**:
- Fix succeeds → Continue to next stage
- Fix introduces regression → Re-run affected prior stages (→ F10)

---

### F2: Stage Check Fails 3 Consecutive Times

**Severity**: High

**Trigger**: The same stage's check fails 3 times in a row after attempted fixes.

**Handling Steps**:
1. STOP. Do not attempt a 4th fix.
2. Compile full context:
   - What the stage is trying to accomplish
   - What the check expects
   - What the check output shows (all 3 attempts)
   - What was attempted each time
3. Escalate to user with this context.
4. Wait for user decision: change approach, skip stage, or abort.

**User Notification Message**:
> This stage has failed verification 3 consecutive times. Here is the context:
> - Stage: [name and goal]
> - Check: [command/method]
> - Attempted fixes: [summary of each attempt]
> - Persistent error: [key error lines]
>
> How would you like to proceed? Options:
> 1. I'll try a different approach
> 2. Skip this stage (note: will affect Final Proof)
> 3. Abort fable mode

---

### F3: Final Proof Fails

**Severity**: Critical

**Trigger**: The Sage's Final Proof check fails — stages do not integrate correctly, requirements are not met, or cross-stage validation fails.

**Handling Steps**:
1. Read the Sage's verdict carefully. Identify the specific failure point.
2. If a specific stage is identified → re-run that stage from the forge (Blacksmith).
3. If a connection between stages is identified → redesign the interface between them.
4. After fix, the Sage re-runs the Final Proof.
5. Do not deliver until the Final Proof passes.

**Recovery Paths**:
- Single stage failure → Re-run that stage → Re-run Final Proof
- Connection failure → Redesign interface → Re-run both stages → Re-run Final Proof
- Multiple failures → Systematic review of all stages → Re-run all affected → Re-run Final Proof

---

### F4: Circular Dependencies

**Severity**: High

**Trigger**: The Cartographer's stage plan contains a circular dependency (A depends on B, B depends on A).

**Detection**:
- During plan review, trace all dependency chains. If any chain loops, this failure is triggered.

**Handling Steps**:
1. STOP. Do not execute a plan with circular dependencies.
2. Return to the Cartographer.
3. Redesign the stages to break the cycle:
   - Split one stage into "before" and "after" parts
   - Merge the dependent stages into one
   - Identify if the dependency is actually bidirectional (both stages need each other's partial output)
4. Re-validate the revised plan.

---

### F5: Parallel Delegation Conflicts

**Severity**: Medium

**Trigger**: In full mode, two parallel stages modify the same file or resource, causing conflicts.

**Detection**:
- After parallel stages complete, check for conflicting modifications.
- If both stages modified the same file, this failure is triggered.

**Handling Steps**:
1. Identify the conflicting stages and the shared resource.
2. Serialize them: re-run one stage, then the other (with awareness of the first's output).
3. Re-run verification checks for both stages.

**Prevention**:
- During planning, the Cartographer should identify shared resources and minimize parallel access.
- Use file-level locking or stage ordering to prevent conflicts.

---

### F6: Scope Creep

**Severity**: Medium

**Trigger**: During execution, the Blacksmith or other agents expand the task beyond the original scope.

**Detection**:
- Compare current work against the original task description.
- If work exceeds the original scope without user approval, this failure is triggered.

**Handling Steps**:
1. STOP the expansion.
2. Document what was added beyond scope.
3. Complete the original plan first.
4. If the expanded scope is valuable, propose it as a separate quest (new fable mode invocation or normal execution).

---

### F7: User Interrupts Mid-Stage

**Severity**: Low

**Trigger**: User requests to stop or change direction while a stage is in progress.

**Handling Steps**:
1. Stop execution immediately.
2. Save current state: which stage was in progress, what was completed, what was the pending check.
3. Summarize to user:
   - Stages completed (with check results)
   - Current stage status (in progress, what remains)
   - What can be resumed later

---

### F8: Verification Tool Unavailable

**Severity**: Medium

**Trigger**: A stage's verification check requires a tool that is not available in the current environment.

**Handling Steps**:
1. Mark the stage as **UNVERIFIED** (not PASS, not FAIL).
2. Document which tool was missing and why the check could not run.
3. Continue to the next stage.
4. In the Final Proof, flag the UNVERIFIED stage and note the risk.
5. In the delivery summary, clearly state which stages were unverified.

---

### F9: Mirror Finds Flaw After Delivery

**Severity**: High

**Trigger**: After delivery, the Mirror's self-review (or user feedback) identifies a flaw that was not caught.

**Handling Steps**:
1. Recall the delivered output.
2. Apply the fix.
3. Re-run the affected stage's Proof check.
4. Re-run the Final Proof if the fix affects integration.
5. Re-deliver with documentation of what was found and fixed.

---

### F10: Prior Stage Regression

**Severity**: High

**Trigger**: Fixing a failing stage causes a previously-passing stage to fail.

**Detection**:
- After fixing Stage N, re-run checks for stages that Stage N's output depended on.
- If any prior check now fails, this failure is triggered.

**Handling Steps**:
1. Identify which prior stage(s) regressed.
2. Diagnose why the fix caused the regression.
3. Fix the regression (may require re-designing the approach).
4. Re-run all affected stage checks.
5. Re-run the Final Proof.

---

### F11: Cartographer Plan Has Gaps

**Severity**: Medium

**Trigger**: During execution, the Blacksmith discovers that the stage plan does not cover some aspect of the task.

**Handling Steps**:
1. STOP execution.
2. Document the gap: what aspect of the task is not covered by any stage.
3. Return to the Cartographer to revise the plan.
4. The Cartographer adds the missing stage(s) with proper checks and dependencies.
5. Resume execution with the revised plan.

---

### F12: Non-Verifiable Output

**Severity**: Medium

**Trigger**: A stage produces output that cannot be verified by any machine-checkable method.

**Handling Steps**:
1. Mark the stage as **UNVERIFIED**.
2. Document why verification is not possible.
3. Continue, but note the risk in the Final Proof.
4. In the delivery summary, clearly state the limitation.

**Examples**:
- "The code is clean" → UNVERifiable. Use lint instead.
- "The design looks good" → UNVERifiable. Use screenshot comparison or accessibility audit.
- "The research is thorough" → Partially verifiable. Use citation coverage check.

---

### F13: Bounce Target Chain Too Long

**Severity**: Medium

**Trigger**: A stage's bounce target chain exceeds 3 hops (e.g., Stage 5 bounces to Stage 3, which bounces to Stage 1, which bounces to plan).

**Detection**:
- During execution, track the bounce chain length.
- If a single failure triggers 3+ bounces, this failure is triggered.

**Handling Steps**:
1. STOP. The bounce chain is not converging.
2. Identify the root cause — why are bounces cascading?
3. Propose a re-plan: either break the dependency chain or merge the bouncing stages into one.
4. Update the stage plan with corrected bounce targets.

**Prevention**:
- During planning, keep bounce targets within 1 hop of the failing stage.
- Avoid chains: `Stage N bounces to Stage N-2 bounces to Stage N-3`.
- Prefer `same` (re-run current stage) or `plan` (full re-plan) over intermediate bounces.

---

### F14: Scope Drift Detected

**Severity**: Medium

**Trigger**: The Blacksmith's scope guard detects that a stage modified a file outside its `Planned Files` list.

**Detection**:
- After executing a stage, compare the set of modified files against the declared `Planned Files`.
- Any file outside the planned list triggers this failure.

**Handling Steps**:
1. Identify the unplanned file(s).
2. Assess whether the change was intentional:
   - **Accidental**: Revert the unplanned change. Re-run the check.
   - **Intentional but unplanned**: Flag for Cartographer revision. Add the file to the plan.
3. If the unplanned file belongs to another stage's planned files → trigger F5 (parallel conflict).
4. Record the drift in the audit trail.

**Prevention**:
- Cartographer should declare all files each stage will touch.
- Use `src/dir/**` globs when a stage touches many files in a directory.
- Review planned files before accepting the stage plan.

---

### F15: Audit Trail Integrity Check Fails

**Severity**: Critical

**Trigger**: The Sage's chain verification command detects a hash mismatch in `.fable/trail/chain.audit.jsonl`.

**Detection**:
- Run the chain verification command (see `references/audit_trail.md`).
- If any entry's `this_hash` does not match the re-computed hash, this failure is triggered.
- If any entry's `prev_hash` does not match the previous entry's `this_hash`, this failure is triggered.

**Handling Steps**:
1. STOP immediately. The audit trail may have been tampered with.
2. Identify the first entry where the chain breaks.
3. Determine if the tampering was:
   - **External** (someone edited the file manually) → Restore from backup or re-run the affected stage.
   - **Internal** (a bug in the recording code) → Fix the bug and re-record affected entries.
   - **Malicious** (unauthorized modification) → Escalate to security.
4. If the chain cannot be repaired, flag the entire run as `TRAIL_COMPROMISED`.
5. The Mirror must review any delivery where the trail is compromised.

**Prevention**:
- Run chain verification as part of each stage completion.
- Store the audit trail in a location with restricted write access.
- Consider backing up `chain.audit.jsonl` after each stage.
