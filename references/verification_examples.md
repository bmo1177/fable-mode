# Verification Examples — Concrete Check Examples

## Overview

This document provides 15+ concrete examples of verification checks, categorized by quality level. Use these as references when defining checks for your stage plan.

---

## Gold Standard Checks

These are exemplary checks: concrete, machine-verifiable, honest, evidence-producing.

### Example 1: TypeScript Compilation
**Stage**: Refactor authentication module
**Check**: `npx tsc --noEmit`
**Pass evidence**: Exit 0, no output
**Fail evidence**: Exit 1, error lines like `src/auth/login.ts:23:5 - error TS2322: Type 'string' is not assignable to type 'number'.`
**Why it's gold**: Specific command, clear pass/fail, produces actionable error output.

### Example 2: Test Suite
**Stage**: Add input validation to registration form
**Check**: `pytest tests/test_registration.py --tb=short`
**Pass evidence**: Exit 0, `12 passed in 0.34s`
**Fail evidence**: Exit 1, `FAILED tests/test_registration.py::test_email_validation - AssertionError: ...`
**Why it's gold**: Specific test file, shows exact failure, exit code is definitive.

### Example 3: Lint Check
**Stage**: Clean up code style across module
**Check**: `npx eslint src/ --format compact`
**Pass evidence**: Exit 0, no output
**Fail evidence**: Exit 1, `src/utils.ts:15:1 - warning: Unexpected any`
**Why it's gold**: Machine-readable output, specific file/line references.

### Example 4: Database Migration
**Stage**: Add user preferences table
**Check**: `prisma migrate status` + verify table exists
**Pass evidence**: `Database schema is up to date`, SELECT returns rows
**Fail evidence**: `2 migrations need to be applied`
**Why it's gold**: Two-part verification: migration status + actual state.

### Example 5: Build Output
**Stage**: Configure Vite build for production
**Check**: `npm run build && ls dist/ | wc -l`
**Pass evidence**: Exit 0, output shows expected file count (e.g., `15`)
**Fail evidence**: Build error or unexpected file count
**Why it's gold**: Verifies both build success and output structure.

---

## Silver Standard Checks

Good checks that are slightly less rigorous but still acceptable.

### Example 6: File Existence
**Stage**: Create component files for new feature
**Check**: `ls src/components/NewFeature/*.tsx | wc -l`
**Pass evidence**: Output shows expected count (e.g., `5`)
**Fail evidence**: Output shows `0` or unexpected count
**Why it's silver**: Verifies files exist but not their content.

### Example 7: Content Presence
**Stage**: Add error handling to API endpoints
**Check**: `grep -r "try {" src/api/ | wc -l`
**Pass evidence**: Count matches expected number of endpoints
**Fail evidence**: Count is lower than expected
**Why it's silver**: Verifies pattern exists but not correctness of implementation.

### Example 8: Dependency Installation
**Stage**: Add new dependencies
**Check**: `node -e "require('zod'); console.log('ok')"`
**Pass evidence**: Output `ok`
**Fail evidence**: Module not found error
**Why it's silver**: Verifies package is installed and importable, not version correctness.

### Example 9: Type Check with Specific File
**Stage**: Fix type errors in specific file
**Check**: `npx tsc --noEmit src/utils/validation.ts`
**Pass evidence**: Exit 0
**Fail evidence**: Type errors listed for that specific file
**Why it's silver**: Targeted check, but doesn't verify the whole project.

### Example 10: Screenshot Comparison
**Stage**: Update component styling
**Check**: Take screenshot, compare to reference image
**Pass evidence**: Visual similarity above threshold
**Fail evidence**: Visual differences detected
**Why it's silver**: Visual verification, but comparison threshold is subjective.

---

## Bronze Standard Checks

Acceptable checks with known limitations. Use only when gold/silver checks are not possible.

### Example 11: Manual Code Review
**Stage**: Refactor complex algorithm
**Check**: Read the code and verify logic
**Pass evidence**: Code reviewer confirms logic is correct
**Fail evidence**: Reviewer identifies issues
**Why it's bronze**: Human judgment, not machine-verifiable. Use as supplementary check only.

### Example 12: Partial Verification
**Stage**: Migrate large codebase
**Check**: Verify one module's migration
**Pass evidence**: Module compiles and tests pass
**Fail evidence**: Errors in that module
**Why it's bronze**: Only verifies a subset of the work. Flag explicitly that full verification is not complete.

---

## Rejected Checks

These are NOT acceptable checks. They cannot fail honestly or produce no evidence.

### Rejected 1: "Does it look right?"
**Why rejected**: Subjective, no pass/fail criterion, no evidence.
**Fix**: Use screenshot comparison, visual regression test, or accessibility audit.

### Rejected 2: "Is the API response correct?"
**Why rejected**: Vague — correct compared to what? No specific assertion.
**Fix**: Compare response against a schema, snapshot, or expected value.

### Rejected 3: "Should work now"
**Why rejected**: Reasoning-based, not tool-verified. The word "should" is the red flag.
**Fix**: Run the actual test/command and verify the output.

### Rejected 4: "Looks good to me"
**Why rejected**: Personal opinion, not a check. Produces no evidence.
**Fix**: Define a specific, machine-verifiable check.

### Rejected 5: "I checked the code and it's fine"
**Why rejected**: Self-assessment without tool verification. The agent is both the worker and the checker.
**Fix**: Use a linter, test runner, or external verification tool.

### Rejected 6: "Tests pass" (without running tests)
**Why rejected**: Claiming without evidence. The check must actually be run.
**Fix**: Run `pytest` or equivalent and capture the output.

---

## Check Definition Template

When defining a check, use this template:

```
Check: [exact command or method]
Pass condition: [specific pass condition — exit code, output pattern, etc.]
Fail condition: [specific fail condition — exit code, error output, etc.]
Evidence: [what the check output shows as evidence]
```

**Example:**
```
Check: pytest tests/test_auth.py --tb=short -q
Pass condition: Exit 0, output contains "passed"
Fail condition: Non-zero exit, output contains "FAILED" or "ERROR"
Evidence: Exit code + count of passed/failed tests
```
