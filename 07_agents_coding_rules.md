# 07. AI Agent System Prompt & Coding Rules (`agents_coding_rules.md`)

> **How to use:** Paste this prompt in the **same chat session** right after finalizing `API.md`. Once generated, the output of this file serves as your master `.cursorrules`, `.windsurfrules`, or custom system instructions for whatever AI coding agent you use to actually write the code. 

---

## Prompt

Now, act as a Principal Software Engineer and AI Coding-Agent Orchestrator specializing in managing autonomous dev tools (like Cursor, Aider, Devin, or Windsurf) within large production codebases.

### CONTEXT
Our foundational blueprint is complete. Use **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, and **`API.md`** as the absolute project source of truth.

### TASK
Create a comprehensive **`agents_coding_rules.md`** (which I will also use as my `.cursorrules` or global agent instruction file). This document must define the strict operational boundaries, coding standards, and behavioral rules that all AI coding agents must follow when building this project. 

Structure `agents_coding_rules.md` with these exact sections:

1. **Prime Directives (Agent Behavior Core)**
   - **Inspect Before Modifying:** Rule mandating the agent to read existing files, grep for usages, and understand the context *before* proposing or executing any code changes.
   - **Scope Discipline:** Strict ban on unrelated refactoring, touching files outside the immediate task, or formatting code not related to the current feature.
   - **No Hallucinations:** Rule enforcing that if a component, database schema, or API endpoint is not in the foundation docs, the agent must ask for permission before inventing it.

2. **Code Quality, Naming & Formatting**
   - Specific naming conventions for files, variables, functions, and database models (e.g., `PascalCase` for React components, `camelCase` for variables, `snake_case` for DB).
   - Rules against code duplication, enforcing DRY (Don't Repeat Yourself) principles without premature abstraction.
   - Prohibition of removing existing developer comments, TODOs, or logging statements unless explicitly instructed.

3. **Project Structure & File Placement**
   - Strict adherence to the directory structure defined in `ARCHITECTURE.md`.
   - Clear boundaries between UI components, business logic/services, and data access layers.

4. **Component & UI Pattern Reuse**
   - Mandate to scan existing UI directories (or `DESIGN.md` tokens) for existing components (buttons, inputs, cards) before building new ones.
   - Strict adherence to the color palette, spacing, and typography locked in `DESIGN.md` (no random hardcoded hex codes or padding values).

5. **Dependency & Package Management**
   - **Zero Rogue Installs:** Absolute ban on installing new npm/pip packages without explicit human approval. The agent must use the approved libraries defined in `ARCHITECTURE.md`.
   - Handling version conflicts and package manager consistency (e.g., exclusively use `pnpm`, `npm`, or `uv`).

6. **Type Safety & Data Validation**
   - Enforcement of strict typing (e.g., `strict: true` in TypeScript). No `any` types.
   - Mandate that all API inputs, form data, and database reads/writes must be validated using the approved schema validation library (e.g., Zod, Pydantic) matching `DATABASE.md`.

7. **Error Handling & State Management**
   - Rules for handling expected vs. unexpected errors based on `API.md`.
   - Enforcing the standard error response envelope for APIs and standardized error boundaries/toast notifications for the UI.

8. **Security, Secrets & Data Privacy**
   - **Never Expose Secrets:** Absolute ban on hardcoding API keys, passwords, or secrets in the codebase. All secrets must use environment variables.
   - Enforcing trust boundaries: client-side code must never execute database queries directly (unless using explicitly configured RLS like Supabase) and must always rely on server authorization.

9. **Database & API Access Rules**
   - Enforcement of exact table names, column types, and foreign key relations from `DATABASE.md`.
   - Rules for using the approved ORM/Query Builder safely (e.g., preventing SQL injection, avoiding N+1 query problems).

10. **Testing & QA Expectations**
    - Where to place test files (`__tests__` or adjacent to files).
    - Guidelines for writing deterministic, flake-free unit/integration tests that mock external dependencies and third-party APIs.

11. **Git, Commit & Changelog Discipline**
    - Commit message formatting rules (e.g., Conventional Commits: `feat:`, `fix:`, `chore:`).
    - Rule emphasizing small, atomic commits over massive single-commit rewrites.

### STRICT RULES
- **No Boilerplate Code:** This file is a rulebook for AI agents, not the application code itself. Do not write actual implementation code.
- **Direct & Unambiguous Tone:** Write these rules using strong imperatives (e.g., "You MUST," "NEVER," "ALWAYS") so coding agents parse them as absolute constraints.

### OUTPUT FORMAT
Return the complete `agents_coding_rules.md` inside a single clean Markdown code block (` ```markdown ... ``` `) ready to save directly.