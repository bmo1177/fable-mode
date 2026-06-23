# Example: Multi-File Refactor Using Fable Mode

## Scenario

The user asks to refactor the authentication module across 5 files, extracting shared logic into a new utility module and updating all consumers.

---

## User Input

```
fable mode on — refactor the authentication module. Extract shared validation logic
from login.ts, register.ts, and resetPassword.ts into a new auth-utils.ts file.
Update all imports and ensure tests still pass.
```

---

## Gate Evaluation

**Cartographer Agent** evaluates:
- **Complexity**: 5 files touched, 2 logical actions (extract + update consumers). Cannot be done in one step. ✓
- **Risk**: Authentication is security-critical, changes affect multiple files, tests must pass. ✓
- **Verdict**: Gate opens. Proceed to planning.

---

## Rite the First: The Cartographer's Map

**Cartographer Agent** produces:

```markdown
# Stage Plan: Auth Module Refactor

**Date**: 2026-06-23
**Mode**: basic
**Total Stages**: 5
**Estimated Complexity**: Medium

---

## Stage Plan

### Stage 1: Create auth-utils.ts with extracted logic

- **Goal**: Extract shared validation functions from login.ts, register.ts, resetPassword.ts into src/auth/auth-utils.ts
- **Expected Output**: New file src/auth/auth-utils.ts containing extractLoginValidation, extractRegisterValidation, extractResetPasswordValidation functions
- **Verification Check**: `npx tsc --noEmit` — exits 0 with no type errors
- **Dependencies**: none

### Stage 2: Update login.ts to use auth-utils

- **Goal**: Replace inline validation in login.ts with import from auth-utils.ts
- **Expected Output**: Modified src/auth/login.ts with import from ./auth-utils
- **Verification Check**: `npx tsc --noEmit` — exits 0 with no type errors
- **Dependencies**: Stage 1

### Stage 3: Update register.ts to use auth-utils

- **Goal**: Replace inline validation in register.ts with import from auth-utils.ts
- **Expected Output**: Modified src/auth/register.ts with import from ./auth-utils
- **Verification Check**: `npx tsc --noEmit` — exits 0 with no type errors
- **Dependencies**: Stage 1

### Stage 4: Update resetPassword.ts to use auth-utils

- **Goal**: Replace inline validation in resetPassword.ts with import from auth-utils.ts
- **Expected Output**: Modified src/auth/resetPassword.ts with import from ./auth-utils
- **Verification Check**: `npx tsc --noEmit` — exits 0 with no type errors
- **Dependencies**: Stage 1

### Stage 5: Run full test suite

- **Goal**: Verify all refactored code works correctly
- **Expected Output**: All tests pass, no regressions
- **Verification Check**: `pytest tests/auth/ --tb=short` — exits 0, all tests pass
- **Dependencies**: Stage 2, Stage 3, Stage 4

---

## Final Proof of Deeds

**Check**: `npx tsc --noEmit && pytest tests/auth/ --tb=short` — both exit 0
**Success Criteria**: No type errors, all auth tests pass, no regressions

---

## Dependency Graph

```
Stage 1 (none) --> Stage 2 (Stage 1)
                 --> Stage 3 (Stage 1)
                 --> Stage 4 (Stage 1)
                         |
                         v
                 Stage 5 (Stage 2, 3, 4)
```
```

**User accepts the plan.**

---

## Rite the Second: The Summoning of Allies

**Blacksmith Agent** executes:

**Mode**: basic (sequential execution)

**Stage 1**: Create auth-utils.ts
- Extracts `extractLoginValidation`, `extractRegisterValidation`, `extractResetPasswordValidation` from the three source files.
- Creates `src/auth/auth-utils.ts` with all three functions.
- **Check**: `npx tsc --noEmit` → Exit 0 ✓ PASS

**Stage 2**: Update login.ts
- Replaces inline validation with `import { extractLoginValidation } from './auth-utils'`.
- **Check**: `npx tsc --noEmit` → Exit 0 ✓ PASS

**Stage 3**: Update register.ts
- Replaces inline validation with `import { extractRegisterValidation } from './auth-utils'`.
- **Check**: `npx tsc --noEmit` → Exit 0 ✓ PASS

**Stage 4**: Update resetPassword.ts
- Replaces inline validation with `import { extractResetPasswordValidation } from './auth-utils'`.
- **Check**: `npx tsc --noEmit` → Exit 0 ✓ PASS

**Stage 5**: Run full test suite
- **Check**: `pytest tests/auth/ --tb=short` → Exit 0, 23 passed ✓ PASS

---

## Rite the Third: The Proof of Deeds (Final Proof)

**Sage Agent** conducts Final Proof:

- **Stage Results**: 5/5 PASS
- **Integration Validation**: Output-input chains verified, cross-stage consistency confirmed
- **Requirements Traceability**: All 3 requirements met (extract logic, update imports, tests pass)
- **Final Proof Check**: `npx tsc --noEmit && pytest tests/auth/ --tb=short` → Exit 0 ✓ PASS
- **Verdict**: PASS

---

## Rite the Fourth: The Mirror of the Guild

**Mirror Agent** self-review:

1. **Hidden Complexity**: None found.
2. **Environment Assumptions**: Assumes Node.js 18+ and pytest available. Stated in delivery.
3. **Brittle Patterns**: None found.
4. **Check Integrity**: All checks stayed honest — none weakened after seeing output.
5. **Scope Adherence**: Stayed in scope — only auth module files touched.
6. **Evidence Completeness**: All PASS verdicts have tool output evidence.

**Mirror Status**: CLEAN

---

## Delivery

```markdown
# Delivery Summary

**Task**: Refactor authentication module — extract shared validation logic
**Mode**: basic
**Date**: 2026-06-23
**Total Stages**: 5
**All Checks Passed**: YES

---

## What Was Done

Extracted shared validation logic from login.ts, register.ts, and resetPassword.ts into a new auth-utils.ts module. Updated all consumers to import from the shared module.

### Stages Completed

| Stage | Output | Verification |
|-------|--------|-------------|
| Stage 1: Create auth-utils.ts | New file with 3 validation functions | PASS |
| Stage 2: Update login.ts | Import from auth-utils | PASS |
| Stage 3: Update register.ts | Import from auth-utils | PASS |
| Stage 4: Update resetPassword.ts | Import from auth-utils | PASS |
| Stage 5: Run tests | 23 tests pass | PASS |

---

## Evidence

- TypeScript compilation: Exit 0, no type errors
- Test suite: 23 passed in 0.45s

---

## Assumptions

- Node.js 18+ is installed
- pytest is available

---

## Scope Boundaries

- No changes to auth test files (tests were already correct)
- No documentation updates

---

## Fable Mode Record

- **Gate**: Passed (5 files, security-critical module)
- **Cartographer**: Plan produced with 5 stages, all with verification checks
- **Blacksmith**: All 5 stages passed
- **Sage**: Final Proof passed
- **Mirror**: Self-review clean
```
