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

---

## Vision-Based Checks (Fable 5)

These checks leverage Claude Fable 5's vision capabilities to verify visual outputs.

### Vision Check 1: Screenshot Comparison
**Stage**: Update component styling
**Check**: Take screenshot of rendered component, compare against reference design
**Pass evidence**: Visual similarity score > 90%, all key elements present
**Fail evidence**: Layout shifts, missing elements, color mismatches
**Why it's vision**: Uses Fable 5's vision to compare actual rendering against expected design

### Vision Check 2: Design Fidelity
**Stage**: Implement new UI from Figma mockup
**Check**: Capture rendered page, compare against Figma export
**Pass evidence**: Component positions, sizes, and colors match within tolerance
**Fail evidence**: Misaligned elements, incorrect spacing, wrong typography
**Why it's vision**: Leverages visual understanding to verify pixel-level accuracy

### Vision Check 3: Responsive Layout
**Stage**: Make component responsive
**Check**: Capture screenshots at 320px, 768px, 1024px, 1440px widths
**Pass evidence**: Layout adapts correctly at all breakpoints
**Fail evidence**: Overflow, truncation, or breakage at any width
**Why it's vision**: Visual verification across multiple viewports

### Vision Check 4: Accessibility Contrast
**Stage**: Improve color contrast for WCAG compliance
**Check**: Capture screenshot, visually verify text is readable against backgrounds
**Pass evidence**: Text clearly readable, contrast appears sufficient
**Fail evidence**: Text blends into background, hard to read
**Why it's vision**: Visual assessment of readability

---

## Self-Verification Checks (Fable 5)

These checks use Fable 5's self-verification capabilities to validate work without external tools.

### Self-Verification 1: Write Your Own Test
**Stage**: Implement a new function
**Check**: After implementing, write a test that exercises the function with edge cases
**Pass evidence**: Test passes, covering normal and edge cases
**Fail evidence**: Test fails, revealing a bug in the implementation
**Why it's self-verification**: The agent writes and runs its own test to verify correctness

### Self-Verification 2: Reasoning Re-derivation
**Stage**: Solve a complex algorithm problem
**Check**: Re-derive the solution independently and compare against original
**Pass evidence**: Both derivations produce identical results
**Fail evidence**: Contradiction found between derivations
**Why it's self-verification**: Agent checks its own reasoning by solving twice

### Self-Verification 3: Output-Against-Goal
**Stage**: Complete a multi-step task
**Check**: Compare final output against original goal statement, criterion by criterion
**Pass evidence**: All goal criteria satisfied with evidence
**Fail evidence**: Missing criteria or insufficient evidence
**Why it's self-verification**: Agent validates its own work against the original requirements

### Self-Verification 4: Code Review
**Stage**: Write a complex implementation
**Check**: Review own code as if reviewing a pull request, looking for bugs and improvements
**Pass evidence**: No issues found, or issues identified and fixed
**Fail evidence**: Issues found but not yet fixed
**Why it's self-verification**: Agent critiques its own work before declaring it done

### Self-Verification 5: Dependency Check
**Stage**: Implement a feature that depends on other modules
**Check**: Verify that all assumptions about dependencies are correct
**Pass evidence**: All dependency assumptions hold
**Fail evidence**: Assumption violated, requiring implementation change
**Why it's self-verification**: Agent validates its own assumptions before proceeding
