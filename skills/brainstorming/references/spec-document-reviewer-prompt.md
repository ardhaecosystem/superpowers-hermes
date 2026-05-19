# Spec Document Reviewer Prompt Template

Use this template when dispatching a spec document reviewer subagent via `delegate_task`.

**Purpose:** Verify the spec is complete, consistent, and ready for implementation planning.

**Dispatch after:** Spec document is written to `docs/superpowers/specs/`

## Template

Fill in the bracketed placeholders, then pass the complete prompt as the `context` parameter to `delegate_task`:

```
description: "Review spec document"
context: |
  You are a spec document reviewer. Verify this spec is complete and ready for planning.

  **Spec to review:** [SPEC_FILE_PATH]

  Read the spec file with `read_file`. Do not trust summaries.

  ## What to Check

  | Category | What to Look For |
  |----------|------------------|
  | Completeness | TODOs, placeholders, "TBD", incomplete sections |
  | Consistency | Internal contradictions, conflicting requirements |
  | Clarity | Requirements ambiguous enough to cause someone to build the wrong thing |
  | Scope | Focused enough for a single plan — not covering multiple independent subsystems |
  | YAGNI | Unrequested features, over-engineering |

  ## Calibration

  **Only flag issues that would cause real problems during implementation planning.**
  A missing section, a contradiction, or a requirement so ambiguous it could be
  interpreted two different ways — those are issues. Minor wording improvements,
  stylistic preferences, and "sections less detailed than others" are not.

  Approve unless there are serious gaps that would lead to a flawed plan.

  ## Output Format

  ## Spec Review

  **Status:** Approved | Issues Found

  **Issues (if any):**
  - [Section X]: [specific issue] - [why it matters for planning]

  **Recommendations (advisory, do not block approval):**
  - [suggestions for improvement]
```

## Hermes dispatch example

```python
delegate_task(
    goal="Review spec document for completeness",
    context="""<paste filled template here>""",
    toolsets=["file"]
)
```

**Reviewer returns:** Status, Issues (if any), Recommendations
