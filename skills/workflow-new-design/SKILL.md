---
name: workflow-new-design
description: Create a new feature design through iterative discussion.
---

## Instructions

1. If `$ARGUMENTS` is provided, use it as the initial idea/topic. Otherwise, ask the user what feature they want to design.

2. Interview the user relentlessly about every aspect of their idea until reaching a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. If a question can be answered by exploring the codebase, explore the codebase instead of asking.

3. Once the design is fully discussed and agreed upon, do the following:
   - Suggest a slug name (English, hyphen-separated) based on the feature content
   - Get today's date in YYYYMMDD format
   - Create the directory `designs/YYYYMMDD-slug/`
   - Write `DESCRIPTION.md` in Chinese, containing at minimum:
     - **功能描述**: What the feature does and why
     - **用户故事**: User stories in "作为...我希望..." format
     - Additional sections as appropriate for the specific design (e.g., 设计细节, 约束条件, 参考引用)
   - If the design references other designs, include references naturally in the relevant sections

4. After writing DESCRIPTION.md, inform the user that the next step is to run `/workflow-plan-design` to generate the implementation plan.
