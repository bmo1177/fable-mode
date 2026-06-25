# Check Types by Domain — Verification Check Patterns

## Overview

This document provides concrete verification check patterns for each domain that fable mode supports. Use these as starting points when defining checks for your stage plan.

---

## Code Domain

### Build & Compilation Checks

| Check | Command | Pass Condition | Fail Condition |
|-------|---------|----------------|----------------|
| TypeScript compilation | `npx tsc --noEmit` | Exit 0, no output | Non-zero exit, error lines |
| Go build | `go build ./...` | Exit 0 | Non-zero exit |
| Rust compilation | `cargo build` | Exit 0 | Compilation errors |
| Java compilation | `mvn compile` | Exit 0 | Compilation errors |
| Python syntax | `python -m py_compile <file>` | Exit 0 | SyntaxError |

### Test Checks

| Check | Command | Pass Condition | Fail Condition |
|-------|---------|----------------|----------------|
| pytest | `pytest --tb=short` | Exit 0, all tests pass | Non-zero exit, failures listed |
| jest | `npx jest --passWithNoTests` | Exit 0 | Failures listed |
| go test | `go test ./...` | Exit 0 | Failures or panics |
| cargo test | `cargo test` | Exit 0 | Failures listed |
| vitest | `npx vitest run` | Exit 0 | Failures listed |

### Lint & Style Checks

| Check | Command | Pass Condition | Fail Condition |
|-------|---------|----------------|----------------|
| ESLint | `npx eslint .` | Exit 0, no warnings | Errors or warnings |
| Ruff | `ruff check .` | Exit 0 | Violations listed |
| Clippy | `cargo clippy -- -D warnings` | Exit 0 | Warnings treated as errors |
| golangci-lint | `golangci-lint run` | Exit 0 | Violations listed |

### Type Safety Checks

| Check | Command | Pass Condition | Fail Condition |
|-------|---------|----------------|----------------|
| TypeScript strict | `npx tsc --noEmit --strict` | Exit 0 | Type errors |
| mypy | `mypy .` | Exit 0 | Type errors |
| pyright | `npx pyright` | Exit 0 | Type errors |

### Integration Checks

| Check | Command | Pass Condition | Fail Condition |
|-------|---------|----------------|----------------|
| API contract | `npx openapi-diff <old> <new>` | No breaking changes | Breaking changes detected |
| Database migration | `prisma migrate status` | Up to date | Pending migrations |
| Dependency audit | `npm audit` | No vulnerabilities | Vulnerabilities found |

---

## Research Domain

### Source Quality Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Source count | Count sources meeting inclusion criteria | ≥ minimum threshold | Below threshold |
| Source diversity | Check source types (journal, conference, book) | ≥ 2 types represented | Single type |
| Currency | Check publication dates | ≥ 50% within 5 years | Majority outdated |
| Peer review status | Check if sources are peer-reviewed | ≥ 70% peer-reviewed | Majority non-reviewed |

### Claim Verification Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Citation coverage | Map each claim to at least one source | 100% coverage | Un-cited claims |
| Contradiction scan | Check for conflicting claims across sources | 0 contradictions (or resolved) | Unresolved contradictions |
| Evidence strength | Grade sources using evidence hierarchy | Average grade ≥ Level III | Average grade below threshold |

### Structure Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Section completeness | Check all required sections present | All sections present | Missing sections |
| Word count | Count words per section | Within target range | Outside range |
| Reference format | Validate citation format | All citations valid format | Invalid citations |

---

## Data Domain

### Schema Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Schema validation | Validate data against schema | All rows valid | Validation errors |
| Type constraints | Check column types match expected | All types match | Type mismatches |
| Null handling | Check required fields are non-null | No unexpected nulls | Null in required field |

### Integrity Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Row count | Compare before/after counts | Expected count (or delta explained) | Unexpected count |
| Uniqueness | Check primary key uniqueness | No duplicates | Duplicates found |
| Referential integrity | Check foreign key references | All references valid | Orphan references |
| Range constraints | Check values within valid ranges | All values in range | Out-of-range values |

### Edge Case Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Empty input | Run with empty dataset | Graceful handling | Crash or error |
| Large input | Run with max expected size | Completes within timeout | Timeout or OOM |
| Special characters | Include unicode, quotes, newlines | Handled correctly | Corruption or errors |
| Boundary values | Include min/max/zero/negative | Correct handling | Incorrect results |

---

## Multi-Step Domain

### Integration Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Output consumption | Verify next stage received correct input | Data matches expected | Missing or malformed data |
| State consistency | Check shared state is coherent | No contradictions | Conflicting state |
| No manual glue | Verify no human intervention needed | Fully automated | Requires manual step |

### Regression Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Prior stage re-run | Re-run earlier stage checks after fix | All still pass | Regression detected |
| Baseline comparison | Compare output to known-good baseline | Matches baseline | Deviation from baseline |
| Side-effect scan | Check for unintended changes | Only intended files changed | Unintended changes |

---

## Visual Verification Domain

### Screenshot Comparison Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Layout match | Take screenshot, compare to reference | Visual similarity > 90% | Layout shifts detected |
| Element presence | Capture rendered UI, check elements | All expected elements visible | Missing elements |
| Color accuracy | Compare color values against design | Colors match within tolerance | Color mismatches |
| Spacing validation | Measure gaps between elements | Spacing within 2px of design | Spacing exceeds tolerance |

### Design Fidelity Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Component alignment | Compare positions against design file | Alignment within 4px | Misaligned components |
| Typography | Check font, size, weight against spec | All text matches spec | Typography deviations |
| Responsive behavior | Test at multiple viewport sizes | Layout adapts correctly | Breakage at any size |
| Dark/light mode | Verify both themes render correctly | Both themes look correct | Theme-specific issues |

### Accessibility Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Color contrast | Run axe-core or Lighthouse | WCAG AA compliance | Contrast violations |
| Keyboard navigation | Test tab order and focus states | All interactive elements reachable | Focus traps or skips |
| Screen reader | Verify ARIA labels and roles | All elements properly labeled | Missing or incorrect labels |
| Alt text | Check images for alt attributes | All images have meaningful alt text | Missing or empty alt text |

### Performance Visual Checks

| Check | Method | Pass Condition | Fail Condition |
|-------|--------|----------------|----------------|
| Loading states | Monitor during page load | Skeletons/loading indicators present | Blank screens |
| Animation smoothness | Record at 60fps, check for jank | Animations stay above 30fps | Visible stuttering |
| Image rendering | Check images load and scale correctly | Images display without distortion | Blurry or broken images |

---

## Choosing the Right Check

When defining a check for your stage, ask:

1. **Can this check genuinely fail?** If no, it is not a check.
2. **Does this check produce evidence?** The output itself is the evidence.
3. **Is this check machine-verifiable?** It must be runnable by a tool, not a human judgment.
4. **Is this check specific?** "Tests pass" is vague. "pytest exits 0 with 47 tests passing" is specific.
5. **Is this check the minimum necessary?** A check that is too comprehensive becomes a bottleneck. A check that is too loose becomes meaningless.
