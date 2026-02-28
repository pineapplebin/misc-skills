# misc-skills

A collection of miscellaneous **Cursor Agent Skills** — reusable instructions that guide the AI when handling specific tasks.

## What are Agent Skills?

Agent Skills are markdown files (with optional YAML front matter) that describe workflows, APIs, and conventions. When a user request matches a skill’s description, the agent reads the skill and follows its instructions to complete the task consistently.

## Skills in this repo

| Skill | Description |
|-------|-------------|
| **generate-gitignore** | Generates `.gitignore` via the [Toptal gitignore](https://www.toptal.com/developers/gitignore). Detects project tech stack from the target directory, merges language, OS, and IDE templates, and writes the result to repo root or a specified path (e.g. monorepo subproject). Use when the user asks to generate, create, or update a gitignore file. |

- **Path:** `skills/SKILL.md` (single file; front matter `name: generate-gitignore`).

## Using these skills

To use a skill in Cursor:

1. Copy or symlink the skill into your Cursor skills directory (e.g. `~/.cursor/skills/` or your project’s skills folder), or  
2. Keep this repo and point Cursor to it if your setup supports custom skill paths.

Skills are invoked automatically when the user’s question matches the skill’s description; you can also reference them explicitly (e.g. “use the generate-gitignore skill”).

## Contributing

Add new skills as markdown files under `skills/` with a clear `name` and `description` in the front matter, and document the workflow in the body.
