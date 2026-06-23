# Final Proof Template

## Purpose

This is the standard output format for the Sage Agent's Final Proof of Deeds. Use this template to verify that all stages integrate correctly and the original requirements are met.

---

## Template

```markdown
# Final Proof of Deeds — Verdict

**Date**: [YYYY-MM-DD]
**Task**: [Original task description]
**Total Stages**: [N]
**Stages Passed**: [N]
**Stages Failed**: [N]
**Stages Unverified**: [N]

---

## Stage Results

| Stage | Status | Check Command | Key Evidence |
|-------|--------|---------------|--------------|
| Stage 1: [name] | PASS | [command] | [output summary] |
| Stage 2: [name] | PASS | [command] | [output summary] |
| Stage 3: [name] | UNVERIFIED | [command] | [reason: tool unavailable] |
| Stage 4: [name] | PASS | [command] | [output summary] |

---

## Integration Validation

### Output-Input Chains

| Dependency | Producer Output | Consumer Input | Match? |
|-----------|----------------|----------------|--------|
| Stage 1 → Stage 2 | [output] | [input] | YES / NO |
| Stage 1 → Stage 3 | [output] | [input] | YES / NO |

### Cross-Stage Consistency

- [ ] All prior stage checks still pass after later fixes
- [ ] No files modified unexpectedly
- [ ] No orphan outputs (outputs produced but not consumed)

### No Manual Glue

- [ ] No human intervention required between stages
- [ ] All stage transitions are automated

---

## Requirements Traceability

| # | Original Requirement | Satisfying Stage | Evidence |
|---|---------------------|-----------------|----------|
| 1 | [requirement] | Stage N | [output] |
| 2 | [requirement] | Stage M | [output] |
| 3 | [requirement] | Stage N + Stage P | [output] |

[Every original requirement must appear here. If any requirement has no satisfying stage, the Final Proof FAILS.]

---

## Final Proof Check

**Command**: [The exact Final Proof check command from the stage plan]

**Output**:
```
[Actual output from running the command]
```

**Exit Code**: [0 = pass, non-zero = fail]

---

## Verdict

**PASS** / **FAIL**

[If PASS: "All stages pass, integration is verified, requirements are met. Ready for Mirror review."]

[If FAIL: "Final Proof failed at [specific point]. Recommended fix: [action]. Returning to Blacksmith for re-execution."]
```

---

## Instructions

1. Fill in all fields after the Blacksmith has completed all stages.
2. Verify EVERY output-input chain — not just some.
3. Every original requirement must be traced to a satisfying stage.
4. Run the Final Proof check as a tool call — do not reason about it.
5. If the Final Proof fails, identify the specific failure point and return to the Blacksmith.
6. Do not deliver until the Final Proof passes.
