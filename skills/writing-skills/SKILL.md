---
name: writing-skills
description: "Use when creating new Hermes skills, editing existing skills, or verifying skills work before deployment"
version: 5.1.0
author: Jesse Vincent (ported to Hermes by Humanth Shashani)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [superpowers, skills, authoring, documentation, quality, workflow]
    homepage: https://github.com/obra/superpowers
    related_skills: [test-driven-development, brainstorming]
---

# Writing Skills

## Overview

**Writing skills IS applying Test-Driven Development to process documentation.**

You write test scenarios (pressure situations), observe baseline behavior without the skill, write the skill document (the "production code"), then verify the skill triggers correctly and guides behavior as intended when loaded in a fresh session.

**Core principle:** If you didn't test the skill in a fresh session, you don't know if it teaches the right thing.

**REQUIRED BACKGROUND:** You MUST understand test-driven-development before using this skill. That skill defines the fundamental RED-GREEN-REFACTOR cycle. This skill adapts TDD to documentation.

## What is a Skill?

A **skill** is a reference guide for proven techniques, patterns, or tools. Skills help future Hermes instances find and apply effective approaches.

**Skills are:** Reusable techniques, patterns, tools, reference guides

**Skills are NOT:** Narratives about how you solved a problem once

## TDD Mapping for Skills

| TDD Concept | Skill Creation |
|-------------|----------------|
| **Test case** | Hermes test scenario (fresh session with task) |
| **Production code** | Skill document (SKILL.md) |
| **Test fails (RED)** | Skill doesn't trigger, or gives wrong guidance |
| **Test passes (GREEN)** | Skill triggers correctly and guides behavior |
| **Refactor** | Close loopholes, improve description for discovery |
| **Write test first** | Test baseline behavior BEFORE writing skill |
| **Watch it fail** | Document exact failures (wrong tool used, missed step) |
| **Minimal code** | Write skill addressing those specific failures |
| **Watch it pass** | Verify skill triggers and guides correctly |
| **Refactor cycle** | Find new edge cases → plug → re-verify |

The entire skill creation process follows RED-GREEN-REFACTOR.

## When to Create a Skill

**Create when:**
- Technique wasn't intuitively obvious to you
- You'd reference this again across projects
- Pattern applies broadly (not project-specific)
- Others would benefit

**Don't create for:**
- One-off solutions
- Standard practices well-documented elsewhere
- Project-specific conventions (put in HERMES.md or AGENTS.md)
- Mechanical constraints (if it's enforceable with regex/validation, automate it—save documentation for judgment calls)

## Skill Types

### Technique
Concrete method with steps to follow (systematic-debugging, using-git-worktrees)

### Pattern
Way of thinking about problems (flatten-with-flags, test-invariants)

### Reference
API docs, syntax guides, tool documentation

## Directory Structure

```
~/.hermes/skills/
  category-name/          # Optional subdirectory for organization
    skill-name/
      SKILL.md              # Main reference (required)
      supporting-file.*     # Only if needed
```

**Notes for Hermes:**
- Hermes discovers skills recursively under `~/.hermes/skills/`
- Subdirectories (categories) are optional but recommended for organization
- Skill names must be unique across all categories
- All skill content is injected at session start based on `description` matching

## SKILL.md Structure

**Frontmatter (YAML):**
- Two required fields: `name` and `description`
- `name`: Use letters, numbers, and hyphens only (no parentheses, special chars)
- `description`: Third-person, describes ONLY when to use (NOT what it does)
  - Start with "Use when..." to focus on triggering conditions
  - Include specific symptoms, situations, and contexts
  - **NEVER summarize the skill's process or workflow**
  - Keep under 500 characters if possible

```markdown
---
name: skill-name-with-hyphens
description: Use when [specific triggering conditions and symptoms]
---

# Skill Name

## Overview
What is this? Core principle in 1-2 sentences.

## When to Use
[Small inline flowchart IF decision non-obvious]

Bullet list with SYMPTOMS and use cases
When NOT to use

## Core Pattern (for techniques/patterns)
Before/after code comparison

## Quick Reference
Table or bullets for scanning common operations

## Implementation
Inline code for simple patterns
Link to file for heavy reference or reusable tools

## Common Mistakes
What goes wrong + fixes

## Real-World Impact (optional)
Concrete results
```

## Hermes Search Optimization (HSO)

**Critical for discovery:** Future Hermes needs to FIND your skill

### 1. Rich Description Field

**Purpose:** Hermes reads descriptions to decide which skills to load for a given task. Make it answer: "Should I read this skill right now?"

**Format:** Start with "Use when..." to focus on triggering conditions

**CRITICAL: Description = When to Use, NOT What the Skill Does**

The description should ONLY describe triggering conditions. Do NOT summarize the skill's process or workflow in the description.

**Why this matters:** When a description summarizes the skill's workflow, the model may follow the description instead of reading the full skill content. A description saying "code review between tasks" might cause the model to do ONE review, even though the skill clearly shows TWO reviews (spec compliance then code quality).

When the description was changed to just "Use when executing implementation plans with independent tasks" (no workflow summary), the model correctly read the flowchart and followed the two-stage review process.

**The trap:** Descriptions that summarize workflow create a shortcut the model will take. The skill body becomes documentation the model skips.

```yaml
# BAD: Summarizes workflow - model may follow this instead of reading skill
description: Use when executing plans - dispatches subagent per task with code review between tasks

# BAD: Too much process detail
description: Use for TDD - write test first, watch it fail, write minimal code, refactor

# GOOD: Just triggering conditions, no workflow summary
description: Use when executing implementation plans with independent tasks in the current session

# GOOD: Triggering conditions only
description: Use when implementing any feature or bugfix, before writing implementation code
```

**Content:**
- Use concrete triggers, symptoms, and situations that signal this skill applies
- Describe the *problem* (race conditions, inconsistent behavior) not *language-specific symptoms* (setTimeout, sleep)
- Keep triggers technology-agnostic unless the skill itself is technology-specific
- If skill is technology-specific, make that explicit in the trigger
- Write in third person (injected into system prompt)
- **NEVER summarize the skill's process or workflow**

```yaml
# BAD: Too abstract, vague, doesn't include when to use
description: For async testing

# BAD: First person
description: I can help you with async tests when they're flaky

# BAD: Mentions technology but skill isn't specific to it
description: Use when tests use setTimeout/sleep and are flaky

# GOOD: Starts with "Use when", describes problem, no workflow
description: Use when tests have race conditions, timing dependencies, or pass/fail inconsistently

# GOOD: Technology-specific skill with explicit trigger
description: Use when using React Router and handling authentication redirects
```

### 2. Keyword Coverage

Use words the model would search for:
- Error messages: "Hook timed out", "ENOTEMPTY", "race condition"
- Symptoms: "flaky", "hanging", "zombie", "pollution"
- Synonyms: "timeout/hang/freeze", "cleanup/teardown/afterEach"
- Tools: Actual commands, library names, file types

### 3. Descriptive Naming

**Use active voice, verb-first:**
- `creating-skills` not `skill-creation`
- `condition-based-waiting` not `async-test-helpers`

### 4. Token Efficiency (Critical)

**Problem:** Skills with descriptions matching common conversation topics get loaded into EVERY conversation. Every token counts.

**Target word counts:**
- Frequently-loaded skills: <200 words total
- Other skills: <500 words (still be concise)

**Techniques:**

**Move details to tool help:**
```bash
# BAD: Document all flags in SKILL.md
hermes config set supports --text, --both, --limit, N

# GOOD: Reference --help
hermes config set supports multiple modes and filters. Run --help for details.
```

**Use cross-references:**
```markdown
# BAD: Repeat workflow details
When searching, use the terminal tool to grep...
[20 lines of repeated instructions]

# GOOD: Reference other skill
Always verify before claiming success. REQUIRED: Use verification-before-completion.
```

**Compress examples:**
```markdown
# BAD: Verbose example (42 words)
Human: "How did we handle authentication errors in React Router before?"
You: I'll search past sessions for React Router authentication patterns.
[Search sessions with query: "React Router authentication error handling 401"]

# GOOD: Minimal example (20 words)
Human: "How did we handle auth errors in React Router?"
You: Searching...
[Search sessions → synthesis]
```

**Eliminate redundancy:**
- Don't repeat what's in cross-referenced skills
- Don't explain what's obvious from command
- Don't include multiple examples of same pattern

**Verification:**
```bash
wc -w ~/.hermes/skills/superpowers/skill-name/SKILL.md
# Frequently-loaded skills: aim for <200 words
# Other skills: aim for <500 words
```

**Name by what you DO or core insight:**
- `condition-based-waiting` > `async-test-helpers`
- `using-skills` not `skill-usage`
- `flatten-with-flags` > `data-structure-refactoring`
- `root-cause-tracing` > `debugging-techniques`

**Gerunds (-ing) work well for processes:**
- `creating-skills`, `testing-skills`, `debugging-with-logs`
- Active, describes the action you're taking

### 5. Cross-Referencing Other Skills

**When writing documentation that references other skills:**

Use skill name only, with explicit requirement markers:
- Good: `**REQUIRED SUB-SKILL:** Use verification-before-completion`
- Good: `**REQUIRED BACKGROUND:** You MUST understand systematic-debugging`
- Bad: `See skills/testing/test-driven-development` (unclear if required)

**Cross-reference format:**
- Hermes auto-loads skills by matching descriptions to conversation context
- If you need to explicitly reference another skill, just use its name
- The model will have access to it if it's already loaded or matches context
- Don't force-load with specific file paths — that wastes context tokens

## Flowchart Usage

```
Need to show information?
  yes → Decision where I might go wrong?
          yes → Small inline flowchart
          no  → Use markdown
  no  → Use markdown
```

**Use flowcharts ONLY for:**
- Non-obvious decision points
- Process loops where you might stop too early
- "When to use A vs B" decisions

**Never use flowcharts for:**
- Reference material → Tables, lists
- Code examples → Markdown blocks
- Linear instructions → Numbered lists
- Labels without semantic meaning (step1, helper2)

## Code Examples

**One excellent example beats many mediocre ones**

Choose most relevant language:
- Testing techniques → TypeScript/JavaScript/Python
- System debugging → Shell/Python
- Data processing → Python

**Good example:**
- Complete and runnable
- Well-commented explaining WHY
- From real scenario
- Shows pattern clearly
- Ready to adapt (not generic template)

**Don't:**
- Implement in 5+ languages
- Create fill-in-the-blank templates
- Write contrived examples

You're good at porting - one great example is enough.

## File Organization

### Self-Contained Skill
```
defense-in-depth/
  SKILL.md    # Everything inline
```
When: All content fits, no heavy reference needed

### Skill with Reusable Tool
```
condition-based-waiting/
  SKILL.md    # Overview + patterns
  example.py  # Working helpers to adapt
```
When: Tool is reusable code, not just narrative

### Skill with Heavy Reference
```
pptx/
  SKILL.md       # Overview + workflows
  pptxgenjs.md   # 600 lines API reference
  ooxml.md       # 500 lines XML structure
  scripts/       # Executable tools
```
When: Reference material too large for inline

## The Iron Law (Same as TDD)

```
NO SKILL WITHOUT TESTING IN A FRESH SESSION FIRST
```

This applies to NEW skills AND EDITS to existing skills.

Write skill before testing? Deploy it untested. Bad idea.
Edit skill without testing? Same violation.

**No exceptions:**
- Not for "simple additions"
- Not for "just adding a section"
- Not for " documentation updates"
- Test means: create fresh Hermes session, check it triggers correctly

**REQUIRED BACKGROUND:** The test-driven-development skill explains why this matters. Same principles apply to documentation.

## Testing Skills in Hermes

Hermes loads skills at session start based on description matching. To test a skill properly, you must verify it triggers in a fresh session.

### Testing Methodology

**1. Write the skill**
Create the SKILL.md with proper YAML frontmatter and description.

**2. Verify discovery**
Check that the skill is discovered by Hermes:
```bash
hermes skills list | grep your-skill-name
```

**3. Test triggering (RED phase)**
Create a test scenario where the skill SHOULD trigger but doesn't, or where the model behaves incorrectly without the skill.

Example: If writing a "using-git-worktrees" skill, open a new Hermes session in a directory with HERMES.md and ask: "Can you help me implement a new feature?" The skill should auto-trigger because the description matches.

**4. Fix and re-test (GREEN phase)**
If the skill doesn't trigger, fix the description keywords or the HERMES.md context. Re-test.

**5. Test content accuracy (REFACTOR phase)**
After triggering, verify the skill's instructions guide behavior correctly. If the model skips steps or follows wrong steps, tighten the content.

### Discipline-Enforcing Skills (rules/requirements)

**Examples:** TDD, verification-before-completion

**Test with:**
- Academic questions: Does the model understand the rules?
- Pressure scenarios: Does the model comply under stress ("hurry up")?
- Multiple pressures combined: time + sunk cost + exhaustion
- Identify rationalizations and add explicit counters

**Success criteria:** Model follows rule under maximum pressure

### Technique Skills (how-to guides)

**Examples:** condition-based-waiting, root-cause-tracing, systematic-debugging

**Test with:**
- Application scenarios: Can the model apply the technique correctly?
- Variation scenarios: Does the model handle edge cases?
- Missing information tests: Do instructions have gaps?

**Success criteria:** Model successfully applies technique to new scenario

### Pattern Skills (mental models)

**Examples:** reducing-complexity, information-hiding concepts

**Test with:**
- Recognition scenarios: Does the model recognize when pattern applies?
- Application scenarios: Can the model use the mental model?
- Counter-examples: Does the model know when NOT to apply?

**Success criteria:** Model correctly identifies when/how to apply pattern

### Reference Skills (documentation/APIs)

**Examples:** API documentation, command references, library guides

**Test with:**
- Retrieval scenarios: Can the model find the right information?
- Application scenarios: Can the model use what it found correctly?
- Gap testing: Are common use cases covered?

**Success criteria:** Model finds and correctly applies reference information

## Common Rationalizations for Skipping Testing

| Excuse | Reality |
|--------|---------|
| "Skill is obviously clear" | Clear to you ≠ clear to the model. Test it. |
| "It's just a reference" | References can have gaps, unclear sections. Test retrieval. |
| "Testing is overkill" | Untested skills have issues. Always. 15 min testing saves hours. |
| "I'll test if problems emerge" | Problems = model can't use skill. Test BEFORE deploying. |
| "Too tedious to test" | Testing is less tedious than debugging bad skill in production. |
| "I'm confident it's good" | Overconfidence guarantees issues. Test anyway. |
| "Academic review is enough" | Reading ≠ using. Test application scenarios. |
| "No time to test" | Deploying untested skill wastes more time fixing it later. |

**All of these mean: Test before deploying. No exceptions.**

## Bulletproofing Skills Against Rationalization

Skills that enforce discipline (like TDD) need to resist rationalization. Models are smart and will find loopholes when under pressure.

### Close Every Loophole Explicitly

Don't just state the rule - forbid specific workarounds:

**Bad example:**
```markdown
Write code before test? Delete it.
```

**Good example:**
```markdown
Write code before test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete
```

### Address "Spirit vs Letter" Arguments

Add foundational principle early:

```markdown
**Violating the letter of the rules is violating the spirit of the rules.**
```

This cuts off entire class of "I'm following the spirit" rationalizations.

### Build Rationalization Table

Capture rationalizations from baseline testing. Every excuse the model makes goes in the table:

```markdown
| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
```

### Create Red Flags List

Make it easy for the model to self-check when rationalizing:

```markdown
## Red Flags - STOP and Start Over

- Code before test
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**
```

### Update HSO for Violation Symptoms

Add to description: symptoms of when you're ABOUT to violate the rule:

```yaml
description: Use when implementing any feature or bugfix, before writing implementation code
```

## RED-GREEN-REFACTOR for Skills

Follow the TDD cycle:

### RED: Test Baseline

Run test scenario WITHOUT the skill in a fresh Hermes session. Document exact behavior:
- What choices did the model make?
- What rationalizations did it use (verbatim)?
- Which pressures triggered violations?

This is "watch the test fail" - you must see what the model naturally does before writing the skill.

### GREEN: Write Minimal Skill

Write skill that addresses those specific rationalizations. Don't add extra content for hypothetical cases.

Test again WITH the skill loaded. Model should now comply.

### REFACTOR: Close Loopholes

Model found new rationalization? Add explicit counter. Re-test until bulletproof.

## Anti-Patterns

### ❌ Narrative Example
"In session 2025-10-03, we found empty projectDir caused..."
**Why bad:** Too specific, not reusable

### ❌ Multi-Language Dilution
example-js.js, example-py.py, example-go.go
**Why bad:** Mediocre quality, maintenance burden

### ❌ Code in Flowcharts
```
step1 [label="import fs"];
step2 [label="read file"];
```
**Why bad:** Can't copy-paste, hard to read

### ❌ Generic Labels
helper1, helper2, step3, pattern4
**Why bad:** Labels should have semantic meaning

## STOP: Before Moving to Next Skill

**After writing ANY skill, you MUST STOP and complete the deployment process.**

**Do NOT:**
- Create multiple skills in batch without testing each
- Move to next skill before current one is verified
- Skip testing because "batching is more efficient"

**The deployment checklist below is MANDATORY for EACH skill.**

Deploying untested skills = deploying untested code. It's a violation of quality standards.

## Skill Creation Checklist (TDD Adapted)

**IMPORTANT: Use `todo` tool to create todo items for EACH checklist item below.**

**RED Phase - Test Baseline:**
- [ ] Create test scenario (task that should trigger the skill)
- [ ] Run scenario WITHOUT skill - document baseline failures verbatim
- [ ] Identify patterns in failures/misguidance

**GREEN Phase - Write Minimal Skill:**
- [ ] Name uses only letters, numbers, hyphens (no parentheses/special chars)
- [ ] YAML frontmatter with required `name` and `description` fields
- [ ] Description starts with "Use when..." and includes specific triggers/symptoms
- [ ] Description written in third person
- [ ] Keywords throughout for search (errors, symptoms, tools)
- [ ] Clear overview with core principle
- [ ] Address specific baseline failures identified in RED
- [ ] Code inline OR link to separate file
- [ ] One excellent example (not multi-language)
- [ ] Run test scenario WITH skill - verify it triggers and guides correctly

**REFACTOR Phase - Close Loopholes:**
- [ ] Identify NEW rationalizations from testing
- [ ] Add explicit counters (if discipline skill)
- [ ] Build rationalization table from all test iterations
- [ ] Create red flags list
- [ ] Re-test until bulletproof

**Quality Checks:**
- [ ] Small flowchart only if decision non-obvious
- [ ] Quick reference table
- [ ] Common mistakes section
- [ ] No narrative storytelling
- [ ] Supporting files only for tools or heavy reference

**Deployment:**
- [ ] Install skill: `cp -r skill-name/ ~/.hermes/skills/superpowers/`
- [ ] Verify discovery: `hermes skills list | grep skill-name`
- [ ] Test trigger in fresh Hermes session
- [ ] Commit skill to git and consider contributing back

## Discovery Workflow

How future Hermes finds your skill:

1. **Encounters problem** ("tests are flaky")
2. **Scans descriptions** (description matches)
3. **Finds SKILL** (loaded by Hermes)
4. **Scans overview** (is this relevant?)
5. **Reads patterns** (quick reference table)
6. **Loads example** (only when implementing)

**Optimize for this flow** - put searchable terms early and often.

## The Bottom Line

**Creating skills IS TDD for process documentation.**

Same Iron Law: No skill without testing in a fresh session first.
Same cycle: RED (baseline) → GREEN (write skill) → REFACTOR (close loopholes).
Same benefits: Better quality, fewer surprises, bulletproof results.

If you follow TDD for code, follow it for skills. It's the same discipline applied to documentation.
