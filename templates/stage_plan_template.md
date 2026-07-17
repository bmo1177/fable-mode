# Stage Plan Template

## Purpose

This is the standard output format for the Cartographer Agent. Fill in all fields before presenting the plan to the user for acceptance.

---

## Template

```markdown
# Stage Plan: [Task Name]

**Date**: [YYYY-MM-DD]
**Mode**: [basic / full / lite]
**Total Stages**: [N]
**Estimated Complexity**: [Low / Medium / High]

---

## Task Description

[Original task description from user]

---

## Stage Plan

### Stage 1: [Stage Name]

**STAGE STATUS**: [PENDING / IN PROGRESS / PASS / FAIL / UNVERIFIED / BOUNCED]

- **Goal**: [What this stage accomplishes]
- **Expected Output**: [Concrete output — file, test result, data structure]
- **Planned Files**: [Exact file paths this stage is expected to create or modify]
- **Verification Check**: [Exact command or method]
- **Check Type**: [standard / vision / self-verification]
- **Dependencies**: none
- **Domain**: [code / research / data / deployment / visual]
- **Bounce Target**: [stage-id to re-enter on failure, or "same" for current stage]

### Stage 2: [Stage Name]

**STAGE STATUS**: [PENDING / IN PROGRESS / PASS / FAIL / UNVERIFIED / BOUNCED]

- **Goal**: [What this stage accomplishes]
- **Expected Output**: [Concrete output]
- **Planned Files**: [Exact file paths this stage is expected to create or modify]
- **Verification Check**: [Exact command or method]
- **Check Type**: [standard / vision / self-verification]
- **Dependencies**: Stage 1
- **Domain**: [code / research / data / deployment / visual]
- **Bounce Target**: [stage-id to re-enter on failure, or "same" for current stage]

### Stage 3: [Stage Name]

**STAGE STATUS**: [PENDING / IN PROGRESS / PASS / FAIL / UNVERIFIED / BOUNCED]

- **Goal**: [What this stage accomplishes]
- **Expected Output**: [Concrete output]
- **Planned Files**: [Exact file paths this stage is expected to create or modify]
- **Verification Check**: [Exact command or method]
- **Check Type**: [standard / vision / self-verification]
- **Dependencies**: Stage 2
- **Domain**: [code / research / data / deployment / visual]
- **Bounce Target**: [stage-id to re-enter on failure, or "same" for current stage]

[Add more stages as needed]

---

## Final Proof of Deeds

**Check**: [Exact command or method to verify the entire quest succeeded]
**Success Criteria**: [What "done" looks like — all stages pass, integration verified, requirements met]

---

## Dependency Graph

```
Stage 1 (none) --> Stage 2 (Stage 1) --> Stage 3 (Stage 2)
                                          |
Stage 4 (none) -------------------------->+
```

[Visual representation of stage dependencies]

---

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| [Risk 1] | [Low/Med/High] | [Low/Med/High] | [How to mitigate] |
| [Risk 2] | [Low/Med/High] | [Low/Med/High] | [How to mitigate] |

---

## Acceptance

- [ ] Plan reviewed by user
- [ ] All stages have concrete outputs
- [ ] All stages have planned files declared
- [ ] All stages have failable checks
- [ ] All checks validated as machine-verifiable (construction-time validation pass)
- [ ] Dependencies are clear and non-circular
- [ ] All bounce targets reference valid stage IDs
- [ ] Final Proof is defined

**Status**: [Pending / Accepted / Rejected]

---

## Checkpoint (for long sessions)

If this task spans multiple sessions, save checkpoints after each stage:

```markdown
# Checkpoint: [Task Name]
**Saved**: [timestamp]
**Current Stage**: [N of M]
**Completed Stages**:
- Stage 1: [name] — PASS (evidence: [key output])
- Stage 2: [name] — PASS (evidence: [key output])
**Context Summary**: [compressed summary of decisions and progress]
**Pending**: [next stage name and goal]
```

Resume from checkpoint: Read the checkpoint file, verify completed stages are still valid, continue from pending stage.
```

---

## Instructions

1. Fill in all fields in the template above.
2. Every stage MUST have a Verification Check that is a concrete, machine-verifiable command.
3. Every stage MUST list Planned Files — exact paths the stage is expected to create or modify.
4. Every stage SHOULD define a Bounce Target for targeted re-entry on failure.
5. Dependencies must be explicit and non-circular.
6. The Final Proof must be defined BEFORE any stage is executed.
7. Present the plan to the user for acceptance before proceeding.
8. If the user requests changes, revise the plan and re-present.
9. Only proceed to execution after the user accepts the plan (or if no user is present, after self-validation).
