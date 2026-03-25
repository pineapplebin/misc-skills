# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **skill collection repository** for Claude Code Agent Skills — reusable markdown instructions that guide Claude when handling specific tasks. Skills are invoked automatically when a user request matches a skill's description.

## Skill Structure

Each skill lives under `skills/<category>/<skill-name>/SKILL.md` and contains:

- **YAML front matter**: `name` and `description` fields
- **Markdown body**: Workflow instructions, API references, and conventions

When invoking a skill, use the `/<category>-<skill-name>` slash command (e.g., `/scaffold-generate-gitignore`). The category prefix in the `name` field prevents naming conflicts as the collection grows.

## Categories

| Category | Purpose |
|----------|---------|
| `workflow` | Development workflow and process guidance |
| `scaffold` | Project framework and architecture setup |

## Architecture

```
skills/
└── <category>-<skill-name>/
    └── SKILL.md   # Single file containing front matter + instructions
```

## Git Conventions

Never run `git commit` unless the user explicitly asks to commit. Complete all file changes first, then wait for instruction.

## Adding a New Skill

1. Choose the appropriate category (`workflow`, `scaffold`, or add a new one to the table above)
2. Create `skills/<category>-<skill-name>/SKILL.md`
3. Add YAML front matter with `name: <category>-<skill-name>` and `description`
4. Document the workflow in the markdown body
