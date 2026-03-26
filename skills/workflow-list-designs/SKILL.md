---
name: workflow-list-designs
description: List all designs and their current status. Use when the user wants to see an overview of all designs, check progress, or find which designs are in-progress or completed. Trigger phrases: "list designs", "show designs", "design status", "what designs do we have", "查看设计进度", "有哪些设计".
---

# List Designs

## Instructions

1. Check that `designs/` exists. If not, inform the user that no designs have been created yet and stop.

2. Scan all subdirectories under `designs/`. For each subdirectory:
   - Check if `DESCRIPTION.md` exists — if not, skip the directory.
   - Check if `PLAN.md` exists and read its frontmatter `status` field.
   - If PLAN.md exists, count the total number of steps and how many are `completed`.

3. Output a summary table:

   ```
   | Design                   | Status      | Progress | Next action                          |
   |--------------------------|-------------|----------|--------------------------------------|
   | 20260318-meta-design     | completed   | 6/6      | —                                    |
   | 20260320-auth-refactor   | in-progress | 2/5      | /workflow-implement-design 20260320-auth-refactor |
   | 20260325-new-feature     | no plan     | —        | /workflow-plan-design 20260325-new-feature        |
   | 20260326-old-idea        | cancelled   | 3/4      | —                                    |
   ```

   Status values:
   - `no plan` — DESCRIPTION.md exists but PLAN.md does not
   - `in-progress` — PLAN.md frontmatter status is `in-progress`
   - `completed` — PLAN.md frontmatter status is `completed`
   - `cancelled` — PLAN.md frontmatter status is `cancelled`

4. After the table, if any designs are `in-progress`, list them with their current state:
   - If the design has a `pending` step, show its title as the next step
   - If no `pending` steps exist but an `in-progress` step is found (interrupted execution), note that the design needs recovery and suggest running `/workflow-implement-design <name>` to handle the stuck step
