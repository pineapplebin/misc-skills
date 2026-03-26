---
name: workflow-plan-design
description: "Generate an implementation plan (PLAN.md) from an existing design's DESCRIPTION.md. This is the second step in the workflow chain: new-design creates DESCRIPTION.md (human contract), plan-design generates PLAN.md (agent contract), and implement-design executes it. Activate via explicit request or as next step after new-design."
---

## Instructions

1. `$ARGUMENTS` should be the design directory name (e.g. `20260318-meta-design`). If not provided, list all directories under `designs/` that contain a DESCRIPTION.md and ask the user which design to plan.

2. If `designs/$ARGUMENTS/` does not exist or `designs/$ARGUMENTS/DESCRIPTION.md` is missing, list available designs under `designs/` and ask the user to confirm or select one.

3. Read `designs/$ARGUMENTS/DESCRIPTION.md` to understand the feature requirements.

4. If `designs/$ARGUMENTS/PLAN.md` already exists, warn the user and ask whether to regenerate or abort.

5. Analyze the DESCRIPTION.md and break the feature down into concrete implementation steps. For each step, consider:
   - What files need to be created or modified
   - Dependencies on other steps (order matters)
   - Concrete technical approach (converting design intent into actionable technical decisions)
   - External API calls, approach decisions, or constraints — capture these in the step's **Description** field

   **Step granularity rules** — each step must satisfy all four:
   - **Single responsibility**: one step does one verifiable thing
   - **Atomic**: either fully done or not started — no half-done state that leaves the codebase inconsistent
   - **Independently confirmable**: the user can verify the outcome after the step completes (run tests, inspect files, manual check)
   - **Module-scoped**: if a change spans multiple modules, split into one step per module
   - **Test steps are separate**: do not mix test changes with feature implementation in the same step

   Too coarse (split further): "implement the entire auth module"
   Too fine (merge up): "add a blank line at line 42"

6. Write `designs/$ARGUMENTS/PLAN.md` with the following structure:

   ```markdown
   ---
   status: in-progress
   created: YYYY-MM-DD
   references:                # optional, list of related design directory names
     - YYYYMMDD-other-design
   ---

   # Implementation Plan: <feature name>

   ## Steps

   ### Step N: <title>

   - **Status**: pending
   - **Description**: <what this step accomplishes; include technical approach, API methods, and key constraints here>
   - **Details**:
     - File: `<file>` — <brief change description>
     - File: `<file>` — <brief change description>
   - **Notes**:        # reserved for implement-design; do not populate during planning
   ```

   **Details format rules**:
   - Each line starts with `File:` followed by the file path, then `—` (em dash), then a brief description of the change
   - One file per line; indent with 2 spaces
   - Details must only contain file paths and brief change descriptions
   - Technical approach and API details belong in **Description**, not Details
   - Do NOT repeat content already stated in the step's **Description**

7. Write the file directly. The user will review via diff. Do not ask for confirmation before writing — the diff review IS the confirmation mechanism.

8. After writing, ask the user to review the plan against this checklist before executing:
   - Are the steps in a logical order (dependencies flow correctly)?
   - Is each step's granularity appropriate — not too coarse, not too fine?
   - Are the file paths in Details accurate?
   - Are there any missing steps?

   Also remind the user to ensure the working tree is clean (`git status`) before running implement-design — each completed step is a natural commit checkpoint.

   When the user is ready, inform them to run `/workflow-implement-design $ARGUMENTS`. The full workflow chain is:
   - workflow-new-design → creates DESCRIPTION.md (Chinese, human contract)
   - workflow-plan-design → creates PLAN.md (English, agent contract)
   - workflow-implement-design → executes PLAN.md step by step
