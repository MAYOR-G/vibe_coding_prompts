# 18. Code Review & Refactoring Prompt (`code_review_and_refactoring_prompts.md`)

> **How to use:** Paste this prompt into your coding agent after completing a major feature milestone or before preparing your production release. It forces the agent to clean up AI-generated bloat, dead code, architectural drift, and performance bottlenecks without breaking working features.

---

## Prompt

Act as a Principal Software Engineer conducting a pre-release architectural code review and surgical refactoring pass.

### CONTEXT
Review the current codebase implementation against our foundational source-of-truth documents: **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**.

**REVIEW SCOPE:** `[ENTIRE CODEBASE / OR SPECIFY TARGET DIRECTORY OR MODULE]`

### WORKFLOW (Execute strictly in this order):

1. **Comprehensive Codebase Audit (Inspect Before Touching):**
   Scan the repository systematically across these 10 quality dimensions:
   - **Architectural Boundary Violations:** Identify business logic or direct database queries leaked into UI components, or files placed in the wrong directories against `ARCHITECTURE.md`.
   - **Design System Drift:** Find hardcoded hex colors, inconsistent spacing, or one-off duplicate UI elements that should use shared components/tokens from `DESIGN.md`.
   - **Dead Code & AI Bloat:** Locate unused imports, orphaned files, commented-out code blocks, leftover `console.log` / debug statements, and unreferenced helper functions.
   - **Duplication & Poor Abstractions:** Identify copy-pasted validation logic, repeated API fetch wrappers, or over-engineered abstractions that add complexity without value.
   - **Type Safety & Schema Gaps:** Find any `any` types, unsafe type assertions (`as unknown as ...`), `@ts-ignore` directives, or missing runtime validation at API boundaries.
   - **Error Handling & Edge Cases:** Spot silent `try/catch` blocks, unhandled promise rejections, missing loading/error UI states, or inconsistent API error formats.
   - **Database & Performance Bottlenecks:** Identify `N+1` query loops, unbounded database fetches missing `.limit()`/pagination, unnecessary client-side re-renders, or oversized client bundle imports.
   - **Dependency Hygiene:** Check `package.json` / `pyproject.toml` for unused, duplicate, or unapproved packages.

2. **Triage Findings by Impact:**
   - Categorize issues into:
     - **Category A (Safe, High-Value Refactors):** Dead code removal, type strengthening, extracting duplicated logic, fixing N+1 queries, and aligning folder/component boundaries.
     - **Category B (Risky / Structural Changes):** Changes requiring database migrations, public API contract alterations, or major state rewrites (flag these for my approval—do **not** auto-apply).

3. **Execute Surgical Refactoring (Category A Only):**
   - Apply clean, incremental refactoring fixes for all Category A issues while strictly preserving external behavior and user-facing functionality.
   - Ensure all refactored code follows the naming, formatting, and modular patterns in `agents_coding_rules.md`.

4. **Verification & Regression Check:**
   - Run the full test suite, type checker (`tsc --noEmit` / `mypy`), linter, and production build command to guarantee zero regressions.

### STRICT RULES
- **Behavior Preservation:** Refactoring must never change working product behavior, break existing routes, or alter database schemas.
- **No Cosmetics-Only Churn:** Do not rewrite working, readable code purely for personal stylistic preference. Every change must demonstrably improve correctness, maintainability, type safety, or performance.
- **Zero New Dependencies:** Do not install new libraries during a refactoring pass.

### OUTPUT & HANDOFF
Once the code review and refactoring pass are verified:
1. Make an atomic Git commit (`refactor: clean up dead code, enforce architecture boundaries, and optimize queries`).
2. Provide a structured **Code Review & Refactoring Report**:
   - **Critical & High-Impact Findings:** Summary of architectural, logic, or performance issues discovered.
   - **Refactorings Executed:** Exact files modified/deleted and why each change was made.
   - **Verification Results:** Output of typecheck, linter, test suite, and build verification.
   - **Deferred Technical Debt (Category B):** Any structural improvements that require my explicit sign-off before touching.