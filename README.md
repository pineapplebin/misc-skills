# misc-skills

A collection of miscellaneous **Claude Code Agent Skills** — reusable instructions that guide the AI when handling specific tasks.

## What are Agent Skills?

Agent Skills are markdown files (with optional YAML front matter) that describe workflows, APIs, and conventions. When a user request matches a skill’s description, the agent reads the skill and follows its instructions to complete the task consistently.

## Using these skills

To use a skill in Claude Code:

1. Copy or symlink the skill into your Cursor skills directory (e.g. `~/.cursor/skills/` or your project’s skills folder), or  
2. Keep this repo and point Cursor to it if your setup supports custom skill paths.

Skills are invoked automatically when the user’s question matches the skill’s description; you can also reference them explicitly (e.g. “use the generate-gitignore skill”).

## Contributing

Add new skills as markdown files under `skills/` with a clear `name` and `description` in the front matter, and document the workflow in the body.
