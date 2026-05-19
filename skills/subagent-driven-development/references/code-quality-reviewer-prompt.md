# Code Quality Reviewer Prompt Template

Use this template when dispatching a code quality reviewer subagent via `delegate_task`.

**Purpose:** Verify implementation is well-built (clean, tested, maintainable)

**Only dispatch after spec compliance review passes.**

## Template

Fill in the bracketed placeholders, then pass the complete prompt as the `context` parameter to `delegate_task`:

```
description: "Code quality review for Task N"
context: |
  You are reviewing code quality for an implementation.

  ## What Was Implemented

  [From implementer's report]

  ## Plan / Requirements

  [Task N from plan-file, or full spec text]

  ## Base Commit

  [commit before task started]

  ## Head Commit

  [current commit after implementation]

  ## Review Checklist

  1. Does each file have one clear responsibility with a well-defined interface?
  2. Are units decomposed so they can be understood and tested independently?
  3. Is the implementation following the file structure from the plan?
  4. Did this implementation create new files that are already large, or significantly grow existing files? (Don't flag pre-existing file sizes — focus on what this change contributed.)
  5. Are names clear and accurate?
  6. Is the code clean, maintainable, and well-tested?
  7. Did the implementer follow TDD discipline if required?

  Read the actual code. Do not trust the implementer's report.

  ## Report Format

  - Strengths
  - Issues by severity: Critical / Important / Minor (with file:line references)
  - Assessment: Approved / Needs fixes
```

## Hermes dispatch example

```python
delegate_task(
    goal="Code quality review for Task 1",
    context="""<paste filled template here>""",
    toolsets=["file", "search"]
)
```
