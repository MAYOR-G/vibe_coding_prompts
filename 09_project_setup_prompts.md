# 09. Project Setup & Initialization Prompt (`project_setup_prompts.md`)

> **How to use:** Paste this prompt into your coding agent (like Cursor, Windsurf, or Aider) when you are ready to actually start building. Ensure the agent has terminal and workspace access so it can execute shell commands, create files, and start the development server.

---

## Prompt

Act as a Senior Full-Stack Engineer responsible for establishing rock-solid, production-ready project foundations.

### CONTEXT
Before executing any commands, you must read our foundational documents: **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, and **`agents_coding_rules.md`**. 

### TASK
Your task is to execute **Phase 0** of our implementation plan. You will initialize the project repository exactly according to the approved architecture.

Execute the following setup steps thoroughly. Use your terminal execution capabilities to run commands and create files.

1. **Git Initialization:** 
   - Initialize a new Git repository (if one does not exist).
   - Create a robust `.gitignore` file tailored to our tech stack.
2. **Framework & Dependency Installation:**
   - Scaffold the base project using the exact framework specified in `ARCHITECTURE.md`.
   - Install **only** the explicitly approved core dependencies (e.g., styling libraries, state management, form validation). 
   - *Strict Rule: Do NOT install any packages that are not mentioned in the architectural documents without asking first.*
3. **Folder Structure Scaffold:**
   - Recreate the exact ASCII directory tree defined in `ARCHITECTURE.md` by creating the necessary empty directories and placeholder `index` files.
4. **Configuration & Typing Setup:**
   - Configure TypeScript (`tsconfig.json`) in strict mode (or the equivalent for our chosen language).
   - Set up linting and formatting rules (e.g., ESLint, Prettier) to enforce code quality from day one.
5. **Environment Variable Architecture:**
   - Create a `.env.example` file mapping out all required keys as defined in `API.md` and `ARCHITECTURE.md`. (Never hardcode real secrets).
6. **Design System & Global Styling:**
   - Configure the styling engine (e.g., Tailwind `tailwind.config.js`, CSS variables, or global stylesheets) using the exact color hex codes, typography, and spacing scales defined in `DESIGN.md`.
7. **Base Routing & Layout:**
   - Set up the root layout/document and the primary routing shell (even if the pages are mostly empty placeholders for now).

### STRICT RULES
- **Inspect First:** If files already exist in this workspace, inspect them first before overwriting.
- **No Feature Creep:** Do NOT build application features, database schemas, or authentication logic yet. This phase is strictly about infrastructure and boilerplate.
- **Verify It Runs:** You must start the development server (e.g., `npm run dev`) and verify it compiles successfully without errors before completing this task.

### OUTPUT & HANDOFF
Once the setup is complete and verified:
1. Make an atomic Git commit with the message `chore: initialize project foundations`.
2. Provide a brief summary of what was installed and configured.
3. Explicitly ask me for permission to move on to the Database & API setup phase.