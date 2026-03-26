---
name: workflow-polish-skill
description: Polish and improve an existing SKILL.md file to meet the Agent Skills specification and LLM best practices. Use when the user asks to polish, improve, refine, or review a SKILL.md file.
---

# Polish SKILL.md

## Step 1 — Read the target file

Ask the user which SKILL.md to polish if not already specified. Read the file in full before proceeding. If the skill has associated `references/` files in the same directory, read those as well.

## Step 2 — Evaluate against the specification

Before asking the user anything, evaluate the file against every criterion below. Record each issue as a specific finding (e.g., "Step 3 uses passive voice: 'it can be configured'") — not just "failed".

### Frontmatter rules

| Field | Rule |
|-------|------|
| `name` | Required. 1–64 chars. Lowercase letters, numbers, hyphens only. No leading, trailing, or consecutive hyphens. Must match the parent directory name. |
| `description` | Required. 1–1024 chars. Must describe **what** the skill does **and when** to use it. Must include specific trigger keywords. |
| `license` | Optional. Short license name or reference to a bundled license file. |
| `compatibility` | Optional. 1–500 chars. Include only if the skill has specific environment requirements (product, system packages, network access). |
| `metadata` | Optional. Arbitrary key-value map for author, version, etc. |
| `allowed-tools` | Optional. Space-delimited list of pre-approved tools. |

### Description quality

For each criterion, record a specific finding if it fails:

- Does the description say what the skill does (not just "helps with X")?
- Does it say when to use it (trigger conditions)?
- Does it contain specific keywords agents would match against user requests?
- Is it under 1024 characters?

### Body content

- Do all sentences use direct imperatives ("Read the file", not "You might want to read the file")?
- Does every "this", "that", "it" have a clear antecedent?
- Are all conventions and constraints spelled out — no implicit knowledge assumed?
- Is the output of each step explicitly fed into the next step that needs it?

### General quality

- Are instructions written for an LLM agent, not a human developer?
- Are all steps explicit and unambiguous?
- Are examples included where inputs/outputs are non-obvious?
- Are common edge cases covered?
- Is SKILL.md under 500 lines? If not, identify what should move to `references/` files.
- Are filler phrases absent ("feel free to", "you can also")?

## Step 3 — Ask about unresolved gaps only

For every finding from Step 2 that cannot be resolved by reading the file or exploring the codebase, ask the user. Group related questions together. For each question, state your recommended answer.

Ask only about:
- Ambiguous intent that the file does not clarify
- Constraints the file omits and you cannot infer
- Whether any steps should move to `references/` files
- Environment requirements, if the skill references external tools, APIs, or system packages

Do not ask about issues you can fix directly based on the spec in Step 2.

## Step 4 — Produce the improved version

Apply all findings from Step 2 and answers from Step 3. Write the full polished SKILL.md in **English**, regardless of the language of the original file. If the user explicitly requests a different language, respect that; otherwise always output English.

Show changes as before/after code blocks for each modified section. Do not show unchanged sections.

Handle these edge cases:
- If the file already meets all criteria, state that explicitly instead of making changes.
- If associated `references/` files need changes, show those diffs separately.

## Step 5 — Write to file

Wait for the user to confirm the diff. Only write the new content to the file after explicit confirmation.
