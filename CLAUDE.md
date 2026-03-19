# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **skill collection repository** for Claude Code Agent Skills — reusable markdown instructions that guide Claude when handling specific tasks. Skills are invoked automatically when a user request matches a skill's description.

## Skill Structure

Each skill lives under `skills/<skill-name>/SKILL.md` and contains:

- **YAML front matter**: `name` and `description` fields
- **Markdown body**: Workflow instructions, API references, and conventions

When invoking a skill, use the `/<skill-name>` slash command (e.g., `/generate-gitignore`).

## Architecture

```
skills/
└── <skill-name>/
    └── SKILL.md   # Single file containing front matter + instructions
```

## Adding a New Skill

1. Create `skills/<skill-name>/SKILL.md`
2. Add YAML front matter with `name` and `description`
3. Document the workflow in the markdown body
