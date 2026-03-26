---
name: workflow-implement-design
description: "Execute the next pending step in a design's implementation plan. This is the third step in the workflow chain: new-design creates DESCRIPTION.md (human contract), plan-design generates PLAN.md (agent contract), and implement-design executes it step by step. Implement-design only modifies Status, Details (file changes), and Notes — never alters the step's Description or structure."
---

## Instructions

1. `$ARGUMENTS` should be the design directory name (e.g. `20260318-meta-design`). If not provided, list all directories under `designs/` that have a PLAN.md with `status: in-progress` and ask the user which one to work on.

2. Read `designs/$ARGUMENTS/PLAN.md`. Verify the frontmatter status is `in-progress`. If status is `completed` or `cancelled`, inform the user and stop.

   To abandon a plan, the user can manually set the frontmatter status to `cancelled` in PLAN.md. This prevents the plan from being accidentally re-executed in future runs.

3. Read `designs/$ARGUMENTS/DESCRIPTION.md` for full context on the feature requirements.

4. Find the first step with status `pending` in the PLAN.md.

   If no `pending` steps exist but an `in-progress` step is found, the previous execution was interrupted. Inform the user which step is stuck and ask how to proceed:
   - **(a) Re-execute** — reset the step's status to `pending` in PLAN.md, then execute it from the beginning
   - **(b) Mark completed** — skip it (use if the user has already done the work manually)
   - **(c) Abort** — stop and let the user investigate

   If no pending or in-progress steps remain, proceed to step 7.

5. Mark that step's status as `in-progress` in PLAN.md, then execute the work described in that step. Follow the step's description and details closely. If anything is unclear or requires a decision, ask the user before proceeding.

6. Inform the user what was done.

   **If the implementation diverged from the planned approach**, determine the root cause before asking the user to confirm:
   - **Minor divergence** (different files touched, small approach adjustment): update the step's **Details** to reflect what was actually done, then proceed to confirmation
   - **Planning issue** (wrong technical approach, missing steps): stop and inform the user to re-run `/workflow-plan-design`
   - **Design issue** (wrong requirements, missing constraints, fundamental misunderstanding): stop and inform the user to update `DESCRIPTION.md` first, then re-run `/workflow-plan-design`

   If the step involves testable changes, remind the user to run relevant tests or manually verify before confirming. Once the user confirms, mark the step's status as `completed` in PLAN.md.

   If additional context, warnings, or non-file-change observations are needed, append them to the step's **Notes** field. Do not use Notes to document file changes (use Details for that).

   Suggest that the user run `git commit` to checkpoint this step before continuing. Do NOT automatically proceed to the next step — wait for the user to run `/workflow-implement-design` again or ask to continue.

7. When all steps are `completed`:
   - Set the frontmatter `status` to `completed`
   - Append a summary section to the end of PLAN.md:

     ```markdown
     ## Summary

     - **Completed**: YYYY-MM-DD
     - **Outcome**: <brief description of what was achieved>
     ```
   - Inform the user the design is complete.

## Modification Rules

The step format is defined by workflow-plan-design and already present in PLAN.md. Only these fields may be modified:

- **Status**: Always editable (pending → in-progress → completed)
- **Details**: Supplement with actual file changes if implementation differed from plan; never remove original entries
- **Notes**: Append observations, warnings, or context; never modify existing notes
- **Description**: Never modify — if the description is wrong, stop and ask the user to re-run `/workflow-plan-design`
