# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **skill collection repository** for Claude Code Agent Skills — reusable markdown instructions that guide Claude when handling specific tasks. Skills are invoked automatically when a user request matches a skill's description.

## Skill Structure

Each skill lives under `skills/<category>/<skill-name>/SKILL.md` and contains:

- **YAML front matter**: `name` and `description` fields
- **Markdown body**: Workflow instructions, API references, and conventions

When invoking a skill, use the `/<skill-name>` slash command (e.g., `/generate-gitignore`). Agents that support nested skill directories will automatically use the category as a namespace prefix.

## Categories

| Category | Purpose |
|----------|---------|
| `workflow` | Development workflow and process guidance |
| `scaffold` | Project framework and architecture setup |

## Architecture

```
skills/
└── <category>/
    └── <skill-name>/
        └── SKILL.md   # Single file containing front matter + instructions
```

## Adding a New Skill

1. Choose the appropriate category (`workflow`, `scaffold`, or add a new one to the table above)
2. Create `skills/<category>/<skill-name>/SKILL.md`
3. Add YAML front matter with `name` and `description`
4. Document the workflow in the markdown body
