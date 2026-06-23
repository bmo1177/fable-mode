# Example: API Migration Using Fable Mode

## Scenario

The user asks to migrate an API from v1 to v2, updating 12 endpoints, their tests, and documentation.

---

## User Input

```
fable mode full — migrate our REST API from v1 to v2. Update all 12 endpoints
to use the new response format, fix the tests, and update the OpenAPI spec.
```

---

## Gate Evaluation

**Cartographer Agent** evaluates:
- **Complexity**: 12 endpoints + tests + docs. Definitely multi-step. ✓
- **Risk**: API breaking changes, many files, must maintain backward compatibility during transition. ✓
- **Verdict**: Gate opens. Proceed to planning.

---

## Rite the First: The Cartographer's Map

**Cartographer Agent** produces:

```markdown
# Stage Plan: API v1 → v2 Migration

**Date**: 2026-06-23
**Mode**: full
**Total Stages**: 8
**Estimated Complexity**: High

---

## Stage Plan

### Stage 1: Create v2 response schema types

- **Goal**: Define TypeScript types for all 12 v2 response formats
- **Expected Output**: src/api/v2/types.ts with type definitions
- **Verification Check**: `npx tsc --noEmit` — exits 0
- **Dependencies**: none

### Stage 2: Migrate endpoints Group A (users, auth)

- **Goal**: Migrate 4 user/auth endpoints to v2 response format
- **Expected Output**: Modified src/api/v2/users.ts and src/api/v2/auth.ts
- **Verification Check**: `npx tsc --noEmit` — exits 0
- **Dependencies**: Stage 1

### Stage 3: Migrate endpoints Group B (products, orders)

- **Goal**: Migrate 4 product/order endpoints to v2 response format
- **Expected Output**: Modified src/api/v2/products.ts and src/api/v2/orders.ts
- **Verification Check**: `npx tsc --noEmit` — exits 0
- **Dependencies**: Stage 1

### Stage 4: Migrate endpoints Group C (admin, reports)

- **Goal**: Migrate 4 admin/report endpoints to v2 response format
- **Expected Output**: Modified src/api/v2/admin.ts and src/api/v2/reports.ts
- **Verification Check**: `npx tsc --noEmit` — exits 0
- **Dependencies**: Stage 1

### Stage 5: Update v1 → v2 compatibility layer

- **Goal**: Ensure v1 clients can still work through a compatibility adapter
- **Expected Output**: src/api/compat/v1-adapter.ts
- **Verification Check**: `npx tsc --noEmit` — exits 0
- **Dependencies**: Stage 2, Stage 3, Stage 4

### Stage 6: Update endpoint tests

- **Goal**: Update all 12 endpoint tests to expect v2 response format
- **Expected Output**: Updated test files in tests/api/
- **Verification Check**: `pytest tests/api/ --tb=short` — exits 0, all tests pass
- **Dependencies**: Stage 2, Stage 3, Stage 4

### Stage 7: Update OpenAPI spec

- **Goal**: Update openapi.yaml to reflect v2 response schemas
- **Expected Output**: Updated openapi.yaml
- **Verification Check**: `npx @redocly/cli lint openapi.yaml` — exits 0, no errors
- **Dependencies**: Stage 1

### Stage 8: Run full integration test

- **Goal**: Verify all endpoints work correctly end-to-end
- **Expected Output**: All integration tests pass
- **Verification Check**: `pytest tests/integration/ --tb=short` — exits 0
- **Dependencies**: Stage 5, Stage 6, Stage 7

---

## Final Proof of Deeds

**Check**: `npx tsc --noEmit && pytest tests/ --tb=short && npx @redocly/cli lint openapi.yaml` — all exit 0
**Success Criteria**: No type errors, all tests pass, OpenAPI spec valid, v1 compat layer works

---

## Dependency Graph

```
Stage 1 (none) --> Stage 2 (Stage 1)
                 --> Stage 3 (Stage 1)
                 --> Stage 4 (Stage 1)
                 --> Stage 7 (Stage 1)

Stage 2,3,4 --> Stage 5 (Stage 2,3,4)
            --> Stage 6 (Stage 2,3,4)

Stage 5,6,7 --> Stage 8 (Stage 5,6,7)
```

---

## Parallel Delegation Plan

**Batch 1** (independent, dispatch in parallel):
- Stage 2: Migrate Group A
- Stage 3: Migrate Group B
- Stage 4: Migrate Group C
- Stage 7: Update OpenAPI spec

**Batch 2** (after Batch 1 completes):
- Stage 5: v1-v2 compat layer
- Stage 6: Update tests

**Batch 3** (after Batch 2 completes):
- Stage 8: Integration test
```

**User accepts the plan.**

---

## Rite the Second: The Summoning of Allies

**Blacksmith Agent** executes in full mode:

**Batch 1 — Parallel dispatch:**

| Stage | Status | Evidence |
|-------|--------|----------|
| Stage 2: Migrate Group A | PASS | tsc: exit 0 |
| Stage 3: Migrate Group B | PASS | tsc: exit 0 |
| Stage 4: Migrate Group C | PASS | tsc: exit 0 |
| Stage 7: Update OpenAPI | PASS | redocly lint: 0 errors |

**Batch 2 — Parallel dispatch:**

| Stage | Status | Evidence |
|-------|--------|----------|
| Stage 5: Compat layer | PASS | tsc: exit 0 |
| Stage 6: Update tests | PASS | pytest: 45 passed |

**Batch 3 — Sequential:**

| Stage | Status | Evidence |
|-------|--------|----------|
| Stage 8: Integration test | PASS | pytest: 12 passed |

---

## Rite the Third: The Proof of Deeds (Final Proof)

**Sage Agent** conducts Final Proof:

- **Stage Results**: 8/8 PASS
- **Integration Validation**: All output-input chains verified
- **Requirements Traceability**: All 3 requirements met (endpoints migrated, tests updated, docs updated)
- **Final Proof Check**: `npx tsc --noEmit && pytest tests/ --tb=short && npx @redocly/cli lint openapi.yaml` → All exit 0 ✓ PASS
- **Verdict**: PASS

---

## Rite the Fourth: The Mirror of the Guild

**Mirror Agent** self-review:

1. **Hidden Complexity**: The v1-v2 compat layer was added during planning — acknowledged as necessary for backward compatibility.
2. **Environment Assumptions**: Node.js 18+, pytest, @redocly/cli available. Stated in delivery.
3. **Brittle Patterns**: None found.
4. **Check Integrity**: All checks stayed honest.
5. **Scope Adherence**: Stayed in scope — exactly 12 endpoints + tests + docs.
6. **Evidence Completeness**: All PASS verdicts have tool output evidence.

**Mirror Status**: CLEAN

---

## Delivery

```markdown
# Delivery Summary

**Task**: Migrate REST API from v1 to v2
**Mode**: full
**Date**: 2026-06-23
**Total Stages**: 8
**All Checks Passed**: YES

---

## What Was Done

Migrated all 12 REST API endpoints to v2 response format, added a v1-v2 compatibility adapter, updated all tests, and updated the OpenAPI specification.

### Stages Completed

| Stage | Output | Verification |
|-------|--------|-------------|
| Stage 1: v2 types | Type definitions for 12 endpoints | PASS |
| Stage 2: Group A | 4 endpoints migrated | PASS |
| Stage 3: Group B | 4 endpoints migrated | PASS |
| Stage 4: Group C | 4 endpoints migrated | PASS |
| Stage 5: Compat layer | v1-v2 adapter | PASS |
| Stage 6: Tests | 45 tests updated and passing | PASS |
| Stage 7: OpenAPI | Spec updated and valid | PASS |
| Stage 8: Integration | 12 integration tests pass | PASS |

---

## Evidence

- TypeScript: Exit 0, no type errors
- Unit tests: 45 passed in 1.2s
- Integration tests: 12 passed in 3.4s
- OpenAPI lint: 0 errors

---

## Assumptions

- Node.js 18+ installed
- pytest available
- @redocly/cli available (npx)

---

## Scope Boundaries

- v1 endpoints remain functional through compat layer
- No performance optimization
- No documentation beyond OpenAPI spec

---

## Fable Mode Record

- **Gate**: Passed (12 endpoints, breaking changes, multi-file)
- **Cartographer**: Plan with 8 stages, 4 parallel dispatches
- **Blacksmith**: All 8 stages passed
- **Sage**: Final Proof passed
- **Mirror**: Self-review clean
```
