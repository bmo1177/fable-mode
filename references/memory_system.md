# Memory System — Cross-Session Learning

## Overview

Fable 5 performs particularly well when it can record lessons from previous runs and reference them. This guide explains how to implement a structured memory system for fable mode.

---

## Memory Structure

Store memories in a dedicated directory: `.fable/memory/`

### File Types

| File | Purpose | Example |
|------|---------|---------|
| `lessons.md` | Lessons learned from past sessions | "This test suite is flaky, retry twice" |
| `corrections.md` | Corrections to wrong assumptions | "Project uses PostgreSQL 15, not 14" |
| `approaches.md` | Confirmed approaches that work | "Functional style preferred over classes" |
| `failures.md` | Failure patterns to avoid | "Auth module breaks when context window fills" |
| `user_prefs.md` | User preferences and decisions | "User prefers verbose output" |

---

## Memory Format

Each memory file follows this format:

```markdown
# [Category Name]

One lesson per entry. One-line summary at the top, then details.

---

## [Lesson Title]

**Date**: [YYYY-MM-DD]
**Source**: [which session, which stage]
**Confidence**: [high/medium/low]
**Last verified**: [YYYY-MM-DD]

[One-line summary]

[Details: what happened, what was learned, what to do differently]

---

## [Next Lesson]
...
```

---

## Recording Lessons

### What to Record

**Record:**
- Corrections to wrong assumptions
- Confirmed approaches that work
- Failure patterns and their fixes
- User preferences and decisions
- Environment details that matter
- Tool quirks and workarounds

**Don't record:**
- Temporary debugging information
- Secrets or credentials
- Information already in the repository (README, code comments)
- Outdated assumptions that were corrected

### Recording Template

When you learn something worth remembering:

```markdown
## [Short title]

**Date**: 2026-06-24
**Source**: fable mode session, Stage 3 (API migration)
**Confidence**: high
**Last verified**: 2026-06-24

[One-line summary of what was learned]

[2-3 sentences explaining the context and why this matters]
```

---

## Using Memories

### Before Planning

When the Cartographer receives a new task:
1. Check `approaches.md` for confirmed approaches
2. Check `failures.md` for known failure patterns
3. Check `user_prefs.md` for user preferences
4. Incorporate into the stage plan

### During Execution

When the Blacksmith encounters a familiar situation:
1. Check `lessons.md` for relevant lessons
2. Check `corrections.md` for wrong assumptions to avoid
3. Apply the lesson if relevant

### After Completion

When the Mirror reviews the process:
1. Record any new lessons learned
2. Update existing lessons if confidence changed
3. Record corrections to previous assumptions

---

## Memory Bootstrapping

To bootstrap memory from existing session history:

```
Reflect on the previous sessions we've had together. Use subagents to identify
core themes and lessons, and store them in .fable/memory/. Make sure you know
to reference .fable/memory/ for future use.
```

### Bootstrap Process

1. **Gather session logs** from previous fable mode runs
2. **Dispatch subagents** to analyze each session
3. **Extract lessons**: What went wrong? What worked? What was surprising?
4. **Deduplicate**: Don't save what's already recorded
5. **Store in appropriate files**: lessons, corrections, approaches, failures

---

## Memory Maintenance

### Update Existing Notes

When you learn something that changes an existing memory:
1. Update the existing note (don't create a duplicate)
2. Update the "Last verified" date
3. Adjust confidence if needed

### Delete Wrong Notes

When a memory turns out to be wrong:
1. Move it to a `deprecated/` subdirectory (don't delete outright)
2. Add a note explaining why it was wrong
3. Keep it for reference (what not to do)

### Review Periodically

Every 10 sessions or so:
1. Review all memory files
2. Remove outdated information
3. Consolidate related lessons
4. Update confidence levels

---

## Fable Mode Integration

### In SKILL.md

Add to the memory integration section:

```
Memory files are stored in .fable/memory/. Check these before planning and
during execution. Record new lessons after completion. Update existing notes
when you learn something that changes them.
```

### In Cartographer

Before planning:
```
Check .fable/memory/approaches.md and .fable/memory/failures.md for relevant
lessons. Incorporate into the stage plan.
```

### In Blacksmith

During execution:
```
If you encounter a familiar pattern, check .fable/memory/lessons.md for
relevant guidance before proceeding.
```

### In Mirror

After review:
```
Record any new lessons learned in .fable/memory/. Update existing notes
if confidence changed. Correct wrong assumptions.
```

---

## Example Memory Entry

```markdown
## Auth module breaks when context window fills

**Date**: 2026-06-20
**Source**: fable mode session, auth refactor (5 stages)
**Confidence**: high
**Last verified**: 2026-06-24

The auth module refactor failed at Stage 4 because the context window filled
up after Stage 3's verbose output. Solution: use context compaction after
each stage, or split auth refactor into smaller stages.

When planning auth-related tasks, budget for context compaction or use
lite mode for single-module refactors.
```
