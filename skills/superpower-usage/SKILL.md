---
name: superpower-usage
description: Use when unsure which superpower skill to apply, needing to chain multiple superpowers together, onboarding to the superpowers methodology, or navigating between the 14 skills. NOT for general coding tasks.
version: 5.1.0
author: Humanth Shashani
license: MIT
metadata:
  hermes:
    tags: [superpowers, onboarding, workflow, methodology, skills]
    related_skills: [using-superpowers, writing-skills]
---

# Superpower Usage Guide

## Overview

This skill maps how to navigate and combine the 14 Superpowers. It does not replace individual skills — it tells you which one to load and in what order.

**Core principle:** Superpowers are designed to chain. No single skill covers a full project lifecycle.

## The 14 Skills — Quick Map

| Skill | Trigger | Position in Lifecycle |
|-------|---------|----------------------|
| **brainstorming** | Need ideas, mockups, visual comparison | Before planning |
| **writing-plans** | Have requirements, need implementation plan | After spec, before code |
| **test-driven-development** | Writing any code | Before every implementation |
| **subagent-driven-development** | Parallel tasks, complex implementation | During implementation |
| **dispatching-parallel-agents** | Multiple independent tasks | During implementation |
| **executing-plans** | Following a written plan step by step | During implementation |
| **systematic-debugging** | Bug, failure, unexpected behavior | Anytime something breaks |
| **using-git-worktrees** | Need clean branch workspace | During branch work |
| **requesting-code-review** | Implementation "done" | After implementation |
| **receiving-code-review** | Got review feedback | After review received |
| **finishing-a-development-branch** | Tests pass, ready to merge | Before merge |
| **verification-before-completion** | About to say "done" | Before every completion claim |
| **writing-skills** | Creating or editing a skill | Skill authoring only |
| **using-superpowers** | General superpowers methodology question | Meta / onboarding |

## Standard Project Lifecycle Pipeline

```
[Task received]
    ↓
brainstorming (if visual / ideas needed)
    ↓
writing-plans (create implementation plan)
    ↓
test-driven-development (write tests first)
    ↓
subagent-driven-development OR dispatching-parallel-agents (implement)
    ↓
executing-plans (follow plan if not already done)
    ↓
verification-before-completion (verify before claiming done)
    ↓
requesting-code-review (get review)
    ↓
receiving-code-review (address feedback)
    ↓
finishing-a-development-branch (merge)
```

**Not every project uses every skill.** A bugfix might skip brainstorming. A docs update might skip TDD. Use judgment.

## How to Load References and Scripts

In Hermes, only SKILL.md auto-loads. Everything else requires explicit action.

**Reference files (.md):**
```python
# When SKILL.md says "see references/implementer-prompt.md"
skill_view(name='subagent-driven-development', file_path='references/implementer-prompt.md')
```

**Scripts (.js, .sh, .py):**
```python
# When SKILL.md says "run scripts/start-server.sh"
terminal(command='bash ~/.hermes/skills/superpowers/brainstorming/scripts/start-server.sh --project-dir .')
```

**Subagent prompt templates:**
Read the reference file with `skill_view`, fill in bracketed placeholders, then dispatch with `delegate_task`.

## Common Workflows

### Workflow A: New Feature
1. **brainstorming** — Explore approaches visually or conceptually
2. **writing-plans** — Turn chosen approach into spec + plan
3. **test-driven-development** — Write tests for the spec
4. **subagent-driven-development** — Implement with review gates
5. **verification-before-completion** — Run full verification
6. **requesting-code-review** — Submit for review

### Workflow B: Bug Fix
1. **systematic-debugging** — Root cause analysis
2. **test-driven-development** — Write repro test first
3. **executing-plans** — Follow minimal fix plan
4. **verification-before-completion** — Verify fix + no regressions
5. **requesting-code-review** — Submit fix

### Workflow C: Skill Authoring
1. **writing-skills** — Learn skill structure
2. **test-driven-development** — Test the skill in a fresh session
3. **verification-before-completion** — Verify before deploying

### Workflow D: Refactor / Cleanup
1. **using-git-worktrees** — Clean workspace (optional)
2. **writing-plans** — Plan the refactor steps
3. **test-driven-development** — Ensure tests cover current behavior
4. **dispatching-parallel-agents** — Parallelize independent refactor tasks
5. **verification-before-completion** — Full regression check

## Skill Chaining Rules

**Always verify between stages:**
- After planning → verify plan matches spec
- After implementation → verification-before-completion
- After review → verify all feedback addressed

**Never skip when:**
- Code is touched → test-driven-development
- Claiming done → verification-before-completion
- Multiple tasks → subagent-driven-development or dispatching-parallel-agents

**Can skip when:**
- Trivial one-liner with no testable behavior → TDD optional (use judgment)
- No visual component → skip brainstorming
- Already have clean branch → skip worktrees

## Red Flags — Wrong Skill Order

| Anti-Pattern | Fix |
|-------------|-----|
| Start coding before planning | STOP → load writing-plans |
| Write tests after code | STOP → load test-driven-development |
| Claim done without verification | STOP → load verification-before-completion |
| Implement everything solo | STOP → load subagent-driven-development |
| Merge without review | STOP → load requesting-code-review |
| Debug by guessing | STOP → load systematic-debugging |

## Meta: Maintaining This Framework

**Adding a new skill:**
1. Follow writing-skills guidelines
2. Test in fresh Hermes session
3. Update this skill's Quick Map table
4. Update any affected workflow pipelines

**When this skill triggers:**
- You asked "which skill should I use?"
- You're unsure how to chain skills
- You're onboarding to Superpowers
- A task spans multiple lifecycle phases

## STOP: Before Choosing a Skill

If you're about to pick a skill, ask:
1. What phase am I in? (plan / implement / debug / review / verify)
2. What skills have I already used this session?
3. What's the next logical step in the pipeline?

If unclear — load this skill (it just triggered).
