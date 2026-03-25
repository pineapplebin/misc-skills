---
name: workflow-implement-design
description: Execute the next pending step in a design's implementation plan. This is the third step in the workflow chain: new-design creates DESCRIPTION.md (human contract), plan-design generates PLAN.md (agent contract), and implement-design executes it step by step. Implement-design only modifies Status, Details (file changes), and Notes — never alters the step's Description or structure.
---

## Instructions

1. `$ARGUMENTS` should be the design directory name (e.g. `20260318-meta-design`). If not provided, list all directories under `designs/` that have a PLAN.md with `status: in-progress` and ask the user which one to work on.

2. Read `designs/$ARGUMENTS/PLAN.md`. Verify the frontmatter status is `in-progress`. If status is `completed` or `cancelled`, inform the user and stop.

3. Read `designs/$ARGUMENTS/DESCRIPTION.md` for full context on the feature requirements.

4. Find the first step with status `pending` in the PLAN.md. If no pending steps remain, proceed to step 7.

5. Mark that step's status as `in-progress` in PLAN.md, then execute the work described in that step. Follow the step's description and details closely. If anything is unclear or requires a decision, ask the user before proceeding.

6. After completing the step's work, inform the user what was done and ask them to confirm. Once confirmed, mark the step's status as `completed` in PLAN.md.

   **If new files were modified or the implementation diverged from the planned approach**, update the step's **Details** to reflect what was actually done. If the divergence is significant (requires changing Description or step structure), stop and inform the user to re-run `/workflow-plan-design` instead of modifying the plan.

   **Notes**: If additional context, warnings, or non-file-change notes are needed, append them to the step's **Notes** field. Notes are for implementation observations only — do not use Notes to document file changes (use Details for that).

   Do NOT automatically proceed to the next step — wait for the user to run `/implement-design` again or ask to continue.

7. When all steps are `completed`:
   - Set the frontmatter `status` to `completed`
   - Append a summary section to the end of PLAN.md:

     ```markdown
     ## Summary

     - **Completed**: YYYY-MM-DD
     - **Outcome**: <brief description of what was achieved>
     ```
   - Inform the user the design is complete.

## Step Format

Each step in PLAN.md must follow this structure:

```markdown
### Step N: <title>

- **Status**: pending | in-progress | completed
- **Description**: <what this step accomplishes>    # immutable — do not modify
- **Details**:
  - File: `<file>` — <brief change description>    # can supplement with actual changes
  - File: `<file>` — <brief change description>
- **Notes**:                                         # optional, can append observations
  - <implementation notes, warnings, or context>
```

**Modification rules**:
- **Status**: Always editable (pending → in-progress → completed)
- **Details**: Can be supplemented with actual file changes; never remove original entries
- **Notes**: Can be appended; never modify existing notes
- **Description**: Never modify — if the description is wrong, stop and ask user to re-run plan-design
