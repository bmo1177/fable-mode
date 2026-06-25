# Example: Research Project Using Fable Mode

## Scenario

The user asks to produce a research report on the impact of AI on higher education, covering literature review, analysis, and report writing.

---

## User Input

```
fable mode on — write a research report on the impact of AI on higher education
quality assurance. Include literature review, analysis of current trends, and
recommendations. Output as a Markdown document.
```

---

## Gate Evaluation

**Cartographer Agent** evaluates:
- **Complexity**: Research + analysis + writing. Multiple logical phases. ✓
- **Risk**: Accuracy matters, sources must be credible, structure must be coherent. ✓
- **Verdict**: Gate opens. Proceed to planning.

**Researcher Agent** available for:
- Source verification and credibility grading
- Literature synthesis and contradiction detection
- Cross-source validation

---

## Rite the First: The Cartographer's Map

**Cartographer Agent** produces:

```markdown
# Stage Plan: AI in Higher Education Research Report

**Date**: 2026-06-24
**Mode**: basic
**Total Stages**: 6
**Estimated Complexity**: Medium
**Effort**: high (planning and verification stages)

---

## Stage Plan

### Stage 1: Define research scope and outline

- **Goal**: Establish the report's scope, key questions, and section structure
- **Expected Output**: Outline document with section headings, key questions, scope boundaries
- **Verification Check**: `grep -c "^##" outline.md` — output ≥ 5 (at least 5 major sections)
- **Dependencies**: none

### Stage 2: Literature search and source collection

- **Goal**: Find and collect 15-25 credible sources on AI in higher education QA
- **Expected Output**: sources.md with annotated bibliography (title, author, year, key finding, credibility grade)
- **Verification Check**: `grep -c "^\*\*Source" sources.md` — output ≥ 15
- **Dependencies**: Stage 1
- **Agent**: Researcher (source verification specialist)

### Stage 3: Source verification and quality grading

- **Goal**: Verify source credibility, flag predatory sources, grade evidence quality
- **Expected Output**: Updated sources.md with credibility grades and verification notes
- **Verification Check**: `grep -c "Grade:" sources.md` — output ≥ 15 (all sources graded)
- **Dependencies**: Stage 2
- **Agent**: Researcher (source verification specialist)

### Stage 4: Analysis and synthesis

- **Goal**: Synthesize findings across sources, identify themes, contradictions, and gaps
- **Expected Output**: analysis.md with thematic synthesis, evidence mapping, gap analysis
- **Verification Check**: `wc -l analysis.md` — output ≥ 200 (substantial analysis)
- **Dependencies**: Stage 3
- **Agent**: Researcher (synthesis specialist)

### Stage 5: Report drafting

- **Goal**: Write the full research report following the outline
- **Expected Output**: report.md — complete research report
- **Verification Check**: `wc -l report.md` — output ≥ 500 (substantial report)
- **Dependencies**: Stage 1, Stage 4

### Stage 6: Review and quality check

- **Goal**: Verify report quality — citations present, structure matches outline, no contradictions
- **Expected Output**: Updated report.md with quality issues fixed
- **Verification Check**: `grep -c "\[1\]" report.md` — output ≥ 15 (inline citations present)
- **Dependencies**: Stage 5

---

## Final Proof of Deeds

**Check**: All sections present, ≥15 inline citations, structure matches outline, no internal contradictions
**Success Criteria**: Report is complete, well-sourced, and structurally sound

---

## Dependency Graph

```
Stage 1 (none) --> Stage 2 (Stage 1) --> Stage 3 (Stage 2) --> Stage 4 (Stage 3)
                                                                 |
Stage 1 ------------------------------------------> Stage 5 (Stage 1, Stage 4)
                                                        |
                                                        v
                                                Stage 6 (Stage 5)
```
```

**User accepts the plan.**

---

## Rite the Second: The Summoning of Allies

**Blacksmith Agent** executes in basic mode (sequential):

**Stage 1**: Define research scope
- Creates outline.md with 7 major sections, 3 key research questions, scope boundaries.
- **Check**: `grep -c "^##" outline.md` → 7 ≥ 5 ✓ PASS

**Stage 2**: Literature search
- Collects 18 sources from academic databases, government reports, and reputable think tanks.
- Creates sources.md with annotated bibliography.
- **Agent**: Researcher (source collection specialist)
- **Check**: `grep -c "^\*\*Source" sources.md` → 18 ≥ 15 ✓ PASS

**Stage 3**: Source verification
- Grades all 18 sources (12 Level I-II, 4 Level III, 2 Level IV).
- Flags 1 source as potentially predatory, replaces with better alternative.
- **Agent**: Researcher (source verification specialist)
- **Check**: `grep -c "Grade:" sources.md` → 18 ≥ 15 ✓ PASS

**Stage 4**: Analysis and synthesis
- Identifies 4 major themes, 2 contradictions, 3 gaps.
- Produces thematic synthesis with evidence mapping.
- **Agent**: Researcher (synthesis specialist)
- **Check**: `wc -l analysis.md` → 247 ≥ 200 ✓ PASS

**Stage 5**: Report drafting
- Writes full report: Introduction, Literature Review, Thematic Analysis, Contradictions & Gaps, Recommendations, Conclusion.
- **Check**: `wc -l report.md` → 623 ≥ 500 ✓ PASS

**Stage 6**: Review and quality check
- Adds 22 inline citations, fixes 1 structural inconsistency, verifies no contradictions.
- **Check**: `grep -c "\[1\]" report.md` → 22 ≥ 15 ✓ PASS

---

## Rite the Third: The Proof of Deeds (Final Proof)

**Sage Agent** conducts Final Proof (effort: high):

- **Stage Results**: 6/6 PASS
- **Integration Validation**: Report follows outline structure, analysis supports report claims, all sources cited
- **Requirements Traceability**: All 3 requirements met (literature review ✓, analysis ✓, recommendations ✓)
- **Final Proof Check**: Manual verification — 7 sections present, 22 citations, structure matches outline ✓ PASS
- **Verdict**: PASS

---

## Rite the Fourth: The Mirror of the Guild

**Mirror Agent** self-review (effort: high, fresh-context verification):

1. **Hidden Complexity**: None found.
2. **Environment Assumptions**: None — all work is self-contained.
3. **Brittle Patterns**: None found.
4. **Check Integrity**: All checks stayed honest. Note: Stage 5 check (`wc -l`) is a proxy for report completeness — actual content quality is verified in Stage 6.
5. **Scope Adherence**: Stayed in scope — report covers AI in higher education QA only.
6. **Evidence Completeness**: All PASS verdicts have evidence.

**Mirror Status**: CLEAN

---

## Memory: Lessons Learned

After delivery, lessons are recorded in `.fable/memory/`:

```
lessons:
  - "Source verification is critical for research projects — always assign Researcher agent"
  - "18 sources provided good coverage — 15-25 is the sweet spot"
  - "Stage 6 (quality check) caught 1 structural inconsistency — always include a review stage"

corrections:
  - "Stage 2 check used '^\*\*Source' pattern — ensure consistent source formatting"

approaches:
  - "Research projects: Stages 2-4 benefit from Researcher agent delegation"
```

---

## Delivery

```markdown
# Delivery Summary

**Task**: Research report on AI in higher education quality assurance
**Mode**: basic
**Date**: 2026-06-24
**Total Stages**: 6
**All Checks Passed**: YES

---

## What Was Done

Produced a complete research report on AI's impact on higher education quality assurance, including literature review (18 sources), thematic analysis, and 5 actionable recommendations.

### Stages Completed

| Stage | Output | Verification | Agent |
|-------|--------|-------------|-------|
| Stage 1: Scope & outline | 7-section outline, 3 research questions | PASS | Cartographer |
| Stage 2: Literature search | 18 annotated sources | PASS | Researcher |
| Stage 3: Source verification | All sources graded, 1 predatory replaced | PASS | Researcher |
| Stage 4: Analysis | 4 themes, 2 contradictions, 3 gaps | PASS | Researcher |
| Stage 5: Report draft | 623-line complete report | PASS | Blacksmith |
| Stage 6: Quality check | 22 citations, structure verified | PASS | Blacksmith |

---

## Evidence

- 18 sources collected and verified
- 4 thematic findings with evidence mapping
- 22 inline citations in final report
- 7 major sections following outline structure

---

## Assumptions

- None — all work is self-contained

---

## Scope Boundaries

- Report focuses on quality assurance (not general AI in education)
- Recommendations are evidence-based, not prescriptive
- No primary data collection (secondary research only)

---

## Fable Mode Record

- **Gate**: Passed (research + analysis + writing)
- **Cartographer**: Plan with 6 stages, sequential execution
- **Blacksmith**: All 6 stages passed
- **Sage**: Final Proof passed (effort: high)
- **Mirror**: Self-review clean (fresh-context verification)
- **Researcher**: Source verification, synthesis, contradiction detection
- **Memory**: Lessons recorded for future research projects
```
