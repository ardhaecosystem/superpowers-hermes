# superpowers-hermes

A port of the [Superpowers methodology](https://github.com/obra/superpowers) by Jesse Vincent, adapted for [Hermes Agent](https://github.com/NousResearch/hermes-agent).

## What Is This?

Superpowers is a set of 15 behavior-shaping skills for AI agents. Each skill enforces proven development practices — TDD, systematic debugging, structured code review, parallel agent dispatch — by embedding discipline directly into the agent's context.

This repository ports the original Claude Code-centric framework to work natively with Hermes Agent's skill discovery and context management system.

## The 15 Skills

| Skill | Purpose |
|-------|---------|
| **using-superpowers** | Bootstrap skill — general methodology and tool references |
| **superpower-usage** | Navigation skill — maps which skill to use when |
| **brainstorming** | Explore approaches visually or conceptually before coding |
| **writing-plans** | Turn requirements into structured implementation plans |
| **test-driven-development** | RED-GREEN-REFACTOR cycle enforcement |
| **subagent-driven-development** | Dispatch fresh subagent per task with two-stage review |
| **dispatching-parallel-agents** | Parallelize independent tasks across subagents |
| **executing-plans** | Execute written plans step by step |
| **systematic-debugging** | Structured root cause tracing and defense-in-depth |
| **using-git-worktrees** | Clean branch workspace management |
| **requesting-code-review** | Structured submission for review |
| **receiving-code-review** | Structured response to feedback |
| **finishing-a-development-branch** | Pre-merge verification and checklist |
| **verification-before-completion** | Never claim "done" without proof |
| **writing-skills** | Author new skills with TDD discipline |

## Installation

```bash
# Clone this repo
git clone https://github.com/ardhaecosystem/superpowers-hermes.git

# Copy skills to your Hermes directory
cp -r superpowers-hermes/skills/* ~/.hermes/skills/

# Verify installation
hermes skills list | grep superpowers
```

## How It Works

Hermes auto-discovers skills from `~/.hermes/skills/` at session start. Skills are loaded when their YAML `description` matches the current task context. Only `SKILL.md` loads automatically — reference files (`references/*.md`) and scripts (`scripts/*`) are pulled on demand via `skill_view` and `terminal` tool calls.

## Key Differences from Original

| Original (obra/superpowers) | This Port |
|---------------------------|-----------|
| Claude Code harness | Hermes Agent harness |
| `.claude-plugin/` bootstrap | `HERMES.md` project-level bootstrap |
| `Skill("name")` manual loading | Description-based auto-discovery |
| `TodoWrite`, `Task()`, `Bash()` tools | `todo`, `delegate_task`, `terminal` tools |
| `your human partner` terminology | Retained — deliberate Superpowers terminology |
| Platform-specific hooks (Cursor, Codex, Gemini) | Hermes-native skill loading only |

## Project Structure

```
superpowers-hermes/
├── HERMES.md              # Project-level bootstrap (auto-loaded by Hermes)
├── AGENTS.md              # Contributor guidelines for AI agents
├── README.md              # This file
├── LICENSE                # MIT
├── skills/                # All 15 ported skills
│   ├── using-superpowers/
│   │   └── SKILL.md
│   ├── superpower-usage/
│   │   └── SKILL.md
│   ├── brainstorming/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   │   ├── spec-document-reviewer-prompt.md
│   │   │   └── visual-companion.md
│   │   └── scripts/       # Node.js brainstorm server
│   │       ├── server.cjs
│   │       ├── frame-template.html
│   │       ├── helper.js
│   │       ├── start-server.sh
│   │       └── stop-server.sh
│   ├── subagent-driven-development/
│   │   ├── SKILL.md
│   │   └── references/      # Subagent prompt templates
│   │       ├── implementer-prompt.md
│   │       ├── spec-reviewer-prompt.md
│   │       └── code-quality-reviewer-prompt.md
│   └── ... (12 more skills)
└── .github/
    ├── PULL_REQUEST_TEMPLATE.md
    └── ISSUE_TEMPLATE/
```

## Usage

Start a new project or open an existing one. Hermes will auto-load `HERMES.md` from the workspace root. Then, whenever a skill's `description` matches the current task context, it auto-loads into the session. This is probabilistic matching — not guaranteed. If a skill doesn't load, pull it manually:

```python
skill_view(name='superpowers:brainstorming')
```

Typical triggers (matching quality depends on description overlap):

| Session start phrase | Skills likely to load |
|---------------------|---------------------|
| "Let's build X" | `brainstorming` → `writing-plans` |
| "How do I fix this?" | `systematic-debugging` |
| "Write tests for this" | `test-driven-development` |
| "Review my code" | `requesting-code-review` |

## Credits

- **Original methodology:** Jesse Vincent ([obra/superpowers](https://github.com/obra/superpowers))
- **Hermes port:** Humanth Shashani
- **License:** MIT

## Contributing

Read `AGENTS.md` before opening any PR. This project follows the same strict quality standards as the original — 94% rejection rate is not a joke.
