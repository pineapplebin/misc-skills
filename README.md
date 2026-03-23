# misc-skills

A collection of miscellaneous **Claude Code Agent Skills** — reusable instructions that guide the AI when handling specific tasks.

## What are Agent Skills?

Agent Skills are markdown files (with optional YAML front matter) that describe workflows, APIs, and conventions. When a user request matches a skill's description, the agent reads the skill and follows its instructions to complete the task consistently.

## Installation

```bash
npx skills add pineapplebin/misc-skills
```

## Using these skills

Skills are invoked automatically when your request matches a skill's description. You can also reference them explicitly, for example:

> "use the generate-gitignore skill"

## Contributing

See `CLAUDE.md` for the skill structure and category conventions.
