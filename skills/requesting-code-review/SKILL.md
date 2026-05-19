---
name: requesting-code-review
description: "Use when completing tasks, implementing major features, or before merging to verify work meets requirements"
version: 5.1.0
author: Jesse Vincent (ported to Hermes by Humanth Shashani)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [superpowers, code-review, quality, workflow]
    homepage: https://github.com/obra/superpowers
    related_skills: [receiving-code-review, subagent-driven-development, verification-before-completion]
---

# Requesting Code Review

Dispatch a code reviewer subagent to catch issues before they cascade. The reviewer gets precisely crafted context for evaluation — never your session's history. This keeps the reviewer focused on the work product, not your thought process, and preserves your own context for continued work.

**Core principle:** Review early, review often.

## When to Request Review

**Mandatory:**
- After each task in subagent-driven development
- After completing major feature
- Before merge to main

**Optional but valuable:**
- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

## How to Request

**1. Get git SHAs:**

```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

Use `terminal` command to run the above.

**2. Dispatch code reviewer subagent:**

Use `delegate_task` tool with a focused prompt including the diff or relevant files.

**Prompt template:**
```markdown
Review the code changes from {BASE_SHA} to {HEAD_SHA}.

Description: {Brief summary of what was built}
Plan/Requirements: {What it should do}

Please review for:
1. Spec compliance — does the code match the requirements?
2. Code quality — readability, test coverage, edge cases
3. Critical issues that block proceeding

Return a structured review with severity levels:
- Critical: must fix before proceeding
- Important: should fix before proceeding
- Minor: nice to have
```

**3. Act on feedback:**
- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

## Example

```
[Just completed Task 2: Add verification function]

You: Let me request code review before proceeding.

[terminal command to get BASE_SHA and HEAD_SHA]

[Dispatch code reviewer subagent via delegate_task]
  goal: "Review the implementation of verifyIndex() and repairIndex()"
  context: """
    Description: Added verifyIndex() and repairIndex() with 4 issue types
    Plan/Requirements: Task 2 from docs/superpowers/plans/deployment-plan.md
    BASE_SHA: a7981ec
    HEAD_SHA: 3df7661
    
    Please review the diff from BASE_SHA to HEAD_SHA. Check spec compliance and code quality.
  """
  toolsets: ["file", "terminal"]

[Subagent returns]:
  Strengths: Clean architecture, real tests
  Issues:
    Important: Missing progress indicators
    Minor: Magic number (100) for reporting interval
  Assessment: Ready to proceed

You: [Fix progress indicators]
[Continue to Task 3]
```

## Integration with Workflows

**Subagent-Driven Development:**
- Review after EACH task
- Catch issues before they compound
- Fix before moving to next task

**Executing Plans:**
- Review after each task or at natural checkpoints
- Get feedback, apply, continue

**Ad-Hoc Development:**
- Review before merge
- Review when stuck

## Red Flags

**Never:**
- Skip review because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback

**If reviewer wrong:**
- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification
