# Delivery Summary Template

## Purpose

This is the standard output format for delivering the final result after the Mirror's self-review is clean. Use this template to provide a clear, honest summary of what was done and what the user should know.

---

## Template

```markdown
# Delivery Summary

**Task**: [Original task description]
**Mode**: [basic / full / lite]
**Date**: [YYYY-MM-DD]
**Total Stages**: [N]
**All Checks Passed**: [YES / NO — if NO, list UNVERIFIED stages]

---

## What Was Done

[1-3 sentence summary of the work completed]

### Stages Completed

| Stage | Output | Verification |
|-------|--------|-------------|
| Stage 1: [name] | [output] | PASS |
| Stage 2: [name] | [output] | PASS |
| Stage 3: [name] | [output] | UNVERIFIED — [reason] |

---

## Evidence

[Concrete evidence that the result is correct — check outputs, test results, integration verification]

---

## Assumptions

[Environment assumptions that may not hold in the user's setup]

- [Assumption 1]: [e.g., "Node.js 18+ is installed"]
- [Assumption 2]: [e.g., "Database is running locally on default port"]
- [If no assumptions: "No environment-specific assumptions made."]

---

## Scope Boundaries

[What was NOT done — explicit exclusions]

- [Exclusion 1]: [e.g., "Performance optimization was not addressed"]
- [Exclusion 2]: [e.g., "Documentation was not updated"]
- [If everything was addressed: "All aspects of the original task were addressed."]

---

## Known Limitations

[Any issues flagged by the Mirror that were documented rather than fixed]

- [Limitation 1]: [e.g., "UNVERIFIED stage — type check could not run due to missing toolchain"]
- [If none: "No known limitations."]

---

## What to Do Next

[Optional — recommended follow-up actions, if any]

1. [Follow-up 1]
2. [Follow-up 2]

---

## Fable Mode Record

- **Gate**: Passed (task met complexity and risk thresholds)
- **Cartographer**: Plan produced with [N] stages, all with verification checks
- **Blacksmith**: All stages executed, [N] passed, [M] unverified
- **Sage**: Final Proof passed — integration verified, requirements traced
- **Mirror**: Self-review clean — no flaws found (or flaws fixed before delivery)
```

---

## Instructions

1. Fill in all fields honestly. Do not overstate results.
2. UNVERIFIED stages must be listed with the reason — do not hide them.
3. Assumptions must be explicit — the user needs to know what may not work in their environment.
4. Scope boundaries must be explicit — the user needs to know what was not done.
5. Keep the summary concise. The user does not need to read the full stage plan — they need the essential information.
