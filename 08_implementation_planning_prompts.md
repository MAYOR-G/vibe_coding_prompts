# 08. Implementation Planning Prompt (`implementation_planning_prompts.md`)

> **How to use:** Paste this prompt into a fresh or existing chat with your coding agent (making sure it has access to all the `.md` files generated so far). This acts as the bridge between planning and building, giving the agent a trackable checklist to follow step-by-step.

---

## Prompt

Now, act as a Principal Technical Lead and Agile Project Manager responsible for executing production software development.

### CONTEXT
Before creating any plans, you must read and deeply understand our foundational documents: **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**.

### TASK
Create a comprehensive **`IMPLEMENTATION_PLAN.md`** that breaks the entire MVP build into small, logical, sequential phases. This will serve as our trackable master checklist during development. Do **NOT** write any application code.

Structure the `IMPLEMENTATION_PLAN.md` with these exact requirements:

1. **Sequential Phase Strategy**
   - **Phase 0: Project Initialization:** Repository setup, env variables, dependency installation, and tooling configuration.
   - **Phase 1: Database & Data Access:** Schema creation, ORM/query setup, migrations, and seed data.
   - **Phase 2: Core API & Auth:** Authentication layer, route wrappers, and core data endpoints.
   - **Phase 3+: Feature-by-Feature (Vertical Slices):** Building out the UI and connecting it to the APIs for each specific feature defined in `PRD.md`.
   - **Final Phase: Polish & QA:** Error states, loading states, testing, and production deployment prep.

2. **Details Required per Phase**
   For every single phase, provide:
   - **Phase Goal:** A concise summary of what will be achieved.
   - **Prerequisites / Dependencies:** What must be completed before starting this phase.
   - **Actionable Tasks:** A granular checklist of steps using Markdown checkboxes (`- [ ]`). Each task must be small enough for an AI agent to implement and verify independently (e.g., an atomic commit).
   - **Target Files / Modules:** The exact file paths (from `ARCHITECTURE.md`) that will be created or modified.
   - **Database / API Changes:** Which specific tables or endpoints are being implemented in this phase.
   - **Testing & Completion Criteria:** How we definitively test and verify that this phase is complete before moving on.

### STRICT RULES
- **No Application Code:** Write the *plan* to write the code, not the code itself.
- **Foundation First:** Never plan a UI task before its underlying database table and API endpoint have been explicitly planned in a prior step.
- **No Rogue Scope:** Do not introduce features, dependencies, or infrastructure not explicitly approved in the foundational documents.
- **Trackability:** Use standard markdown checkboxes (`- [ ]`) so I can literally check them off as we build.

### OUTPUT FORMAT
Return the complete implementation plan inside a single clean Markdown code block (` ```markdown ... ``` `) ready to save directly as `IMPLEMENTATION_PLAN.md`. 

At the very end of your response (outside the code block), explicitly tell me the exact prompt I should give you next to execute **Phase 0, Task 1**.