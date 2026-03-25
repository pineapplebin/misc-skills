---
name: workflow-polish-skill
description: Polish and improve an existing SKILL.md file to meet the Agent Skills specification and LLM best practices. Use when the user asks to polish, improve, refine, or review a SKILL.md file.
---

# Polish SKILL.md

## Step 1 — Read the target file

Ask the user which SKILL.md to polish if not already specified. Read the file in full before proceeding.

## Step 2 — Grill the user for missing context

Interview the user relentlessly about every aspect of the skill until you reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one.

For each question, provide your recommended answer. If a question can be answered by reading the existing SKILL.md content or by exploring the codebase, do so yourself instead of asking.

Focus your questions on gaps that affect the quality of the output:

- What exactly does this skill do, and in what situations should an agent activate it?
- What are the specific trigger phrases or keywords a user might say?
- Are there edge cases or constraints the current instructions don't cover?
- Is there a preferred output format or style for the skill's results?
- Should any steps be broken into sub-steps or moved to a `references/` file?

If the skill body references external tools, APIs, or system packages, also ask about environment requirements (compatibility). Skip this question for skills that do not involve tool usage.

## Step 3 — Evaluate against the specification

Check every aspect below and note each issue before producing the improved version.

### Frontmatter rules

| Field | Rule |
|-------|------|
| `name` | Required. 1–64 chars. Lowercase letters, numbers, hyphens only. No leading, trailing, or consecutive hyphens. Must match the parent directory name. |
| `description` | Required. 1–1024 chars. Must describe **what** the skill does **and when** to use it. Must include specific trigger keywords. |
| `license` | Optional. Short license name or reference to a bundled license file. |
| `compatibility` | Optional. 1–500 chars. Include only if the skill has specific environment requirements (product, system packages, network access). |
| `metadata` | Optional. Arbitrary key-value map for author, version, etc. |
| `allowed-tools` | Optional. Space-delimited list of pre-approved tools. |

### Description quality checklist

- [ ] Describes what the skill does (not just "helps with X")
- [ ] Describes when to use it (trigger conditions)
- [ ] Contains specific keywords agents would match against user requests
- [ ] Under 1024 characters

### Body content checklist

- [ ] Uses direct imperative sentences ("Read the file", not "You might want to read the file")
- [ ] Contains no ambiguous references (every "this", "that", "it" has a clear antecedent)
- [ ] Does not assume implicit knowledge the LLM would not have — spell out every convention and constraint
- [ ] Dependencies between steps are explicit (state what output from step N feeds into step N+1)

### General quality checklist

- [ ] Instructions are written for an LLM agent, not a human developer
- [ ] Steps are explicit and unambiguous — no implied knowledge
- [ ] Includes examples of inputs and expected outputs where useful
- [ ] Covers common edge cases
- [ ] Uses progressive disclosure: keep SKILL.md under 500 lines; move detailed reference material to `references/` files
- [ ] Avoids filler phrases ("feel free to", "you can also") — use direct imperatives

## Step 4 — Produce the improved version

Write the full polished SKILL.md content in **English**, regardless of the language of the original file. If the user explicitly requests a different language, respect that; otherwise always output English.

Apply all findings from Step 3 and all context gathered in Step 2.

Then show the changes between the original and the improved version.

## Step 5 — Write to file

Wait for the user to confirm the diff. Only write the new content to the file after explicit confirmation.
