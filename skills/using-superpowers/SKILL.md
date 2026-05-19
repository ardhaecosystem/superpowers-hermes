---
name: using-superpowers
description: "Use when starting any conversation - establishes how to find and use skills, requiring skill loading before ANY response including clarifying questions"
version: 5.1.0
author: Jesse Vincent (ported to Hermes by Humanth Shashani)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [superpowers, workflow, methodology, skills, bootstrap]
    homepage: https://github.com/obra/superpowers
    related_skills: [brainstorming, writing-plans, test-driven-development]
---

# Using Superpowers

## Introduction

Superpowers is a complete software development methodology built on composable skills. The skills trigger automatically in Hermes based on task matching — you don't need to do anything special. Your agent just has Superpowers.

## When This Skill Applies

This skill applies at the start of every conversation. It establishes the discipline of checking for skills before any action.

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

## The Rule

**Load relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means that you should load it to check. If a loaded skill turns out to be wrong for the situation, you don't need to use it.

## Skill Access in Hermes

- Skills are auto-discovered from `~/.hermes/skills/` at session start.
- Hermes injects matching skills into context automatically based on YAML frontmatter `description`.
- **Auto-injection is the primary mechanism.** You do NOT need to manually invoke skills.
- To read a support file (reference, template, script) linked from an already-loaded skill: `skill_view(name='superpowers:<skill-name>', file_path='references/<file>.md')`.
- To list available skills: `skills_list()` or `hermes skills list` in terminal.

## Process Flow

```
User message received
  → About to respond?
    → Already brainstormed?
      no  → Load brainstorming skill
      yes → Might any skill apply?
        yes, even 1% → Load skill via skill_view or auto-injection
          → Announce: "Using [skill] to [purpose]"
          → Has checklist?
            yes → Create todo items per checklist
            no  → Follow skill exactly
        definitely not → Respond (including clarifications)
```

**The terminal state is following the skill and responding.** Do NOT skip skill checks.

## Red Flags

These thoughts mean STOP — you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept != using the skill. Load it. |

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) — these determine HOW to approach the task
2. **Implementation skills second** (frontend-design, mcp-builder) — these guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill Types

- **Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.
- **Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.

## Full Skill Library

### Testing
- **test-driven-development** — RED-GREEN-REFACTOR cycle (includes testing anti-patterns reference)

### Debugging
- **systematic-debugging** — 4-phase root cause process (includes root-cause-tracing, defense-in-depth, condition-based-waiting techniques)
- **verification-before-completion** — Ensure it's actually fixed

### Collaboration
- **brainstorming** — Socratic design refinement
- **writing-plans** — Detailed implementation plans
- **executing-plans** — Batch execution with checkpoints
- **dispatching-parallel-agents** — Concurrent subagent workflows
- **requesting-code-review** — Pre-review checklist
- **receiving-code-review** — Responding to feedback
- **using-git-worktrees** — Parallel development branches
- **finishing-a-development-branch** — Merge/PR decision workflow
- **subagent-driven-development** — Fast iteration with two-stage review (spec compliance, then code quality)

### Meta
- **writing-skills** — Create new skills following best practices (includes testing methodology)
- **using-superpowers** — Introduction to the skills system (this skill)

## The Basic Workflow

1. **brainstorming** — Activates before writing code. Refines rough ideas through questions, explores alternatives, presents design in sections for validation. Saves design document.

2. **using-git-worktrees** — Activates after design approval. Creates isolated workspace on new branch, runs project setup, verifies clean test baseline.

3. **writing-plans** — Activates with approved design. Breaks work into bite-sized tasks (2-5 minutes each). Every task has exact file paths, complete code, verification steps.

4. **subagent-driven-development** or **executing-plans** — Activates with plan. Dispatches fresh subagent per task with two-stage review, or executes in batches with human checkpoints.

5. **test-driven-development** — Activates during implementation. Enforces RED-GREEN-REFACTOR: write failing test, watch it fail, write minimal code, watch it pass, commit. Deletes code written before tests.

6. **requesting-code-review** — Activates between tasks. Reviews against plan, reports issues by severity. Critical issues block progress.

7. **finishing-a-development-branch** — Activates when tasks complete. Verifies tests, presents options (merge/PR/keep/discard), cleans up worktree.

**You MUST check for relevant skills before any task.** Mandatory workflows, not suggestions.
