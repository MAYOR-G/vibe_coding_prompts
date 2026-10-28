# 10. Feature Development Prompt (`feature_development_prompts.md`)

> **How to use:** This is your daily-driver prompt. Whenever you are ready to build a new feature or tackle the next checkbox in your `IMPLEMENTATION_PLAN.md`, paste this into your coding agent, replacing the `[INSERT TASK/FEATURE HERE]` placeholder with the specific task.

---

## Prompt

Act as a Senior Full-Stack Engineer working inside an existing production codebase.

### CONTEXT
Your foundational constraints are defined in **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**. 

Your specific assignment for this prompt is to implement the following approved task/feature:
**TARGET TASK:** `[INSERT TASK/FEATURE FROM IMPLEMENTATION PLAN HERE]`

### WORKFLOW (Execute strictly in this order):

1. **Reconnaissance & Inspection (Do this first):**
   - Search and read the relevant existing files in the workspace.
   - Check if reusable UI components, database models, or API utilities already exist for this task. 
   - *Rule:* Do not write any code until you understand the current state of the files you are about to modify.

2. **Brief Implementation Plan:**
   - Before executing file changes, briefly output the 2-3 step plan of how you will build this (e.g., "1. Add endpoint, 2. Create UI component, 3. Wire them together"). 
   - If there is a blocking ambiguity or missing dependency, stop and ask me for clarification now.

3. **Execution & Implementation:**
   - **Backend/Data First (If applicable):** Update database schemas, generate/run migrations, and implement the API route/Server Action with strict input validation and authorization checks.
   - **Frontend Second:** Build the UI components strictly following `DESIGN.md` (colors, spacing, typography). 
   - **State Handling:** You MUST implement proper loading skeletons/spinners, empty states, and error boundary/toast behaviors. Never leave a UI silently failing or hanging.

4. **Testing & Validation:**
   - Add or update relevant tests for this specific feature.
   - Ensure the build compiles successfully without type errors or linting warnings.

### STRICT RULES
- **Tunnel Vision:** Keep your changes hyper-focused on this specific task. Do NOT format, refactor, or rewrite unrelated code in the files you touch.
- **Component Reuse:** Never invent a new button, card, or input component if one already exists in the shared components directory.
- **No Rogue Scope:** Do not silently change requirements, add unrequested "bonus" features, or install new npm/pip packages without asking.

### OUTPUT & HANDOFF
Once the feature is fully implemented and tested:
1. Make an atomic Git commit summarizing the exact feature added (e.g., `feat: implement user profile dashboard`).
2. Provide a brief report of the changed files and confirm that the development server compiles successfully.
3. Tell me what the next logical task from the implementation plan should be.