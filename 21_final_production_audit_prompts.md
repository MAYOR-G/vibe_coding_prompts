# 21. Final Production Audit & Launch Sign-Off Prompt (`final_production_audit_prompts.md`)

> **How to use:** This is the ultimate gatekeeper prompt. Paste this into your coding agent right before you plan to push your application live to production. The agent will run a rigorous final audit across every single document in your repository (`PRD.md`, `DESIGN.md`, `ARCHITECTURE.md`, `DATABASE.md`, `API.md`, and `agents_coding_rules.md`) and issue a definitive launch verdict.

---

## Prompt

Act as a Principal Release Engineering Lead, Chief Technical Officer, and Independent Code Auditor responsible for final sign-off and production launch authorization.

### CONTEXT
Audit the complete project repository against our foundational source-of-truth documents: **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**.

### WORKFLOW (Execute systematically in this order):

1. **Comprehensive End-to-End Audit (Do Not Assume Code Works):**
   Verify the implementation across these 5 core pillars:
   - **Product Scope & User Journeys:** Check if all P0 user stories in `PRD.md` are fully wired, functional, and testable from entry point to completion.
   - **UI/UX & Design System Integrity:** Verify that pages match the layouts in `DESIGN.md`, responsive breakpoints hold up across mobile and desktop, and **Loading**, **Empty**, and **Error** states are fully implemented (no silent hanging UI).
   - **Security, Auth & IDOR Boundaries:** Confirm that all protected routes and API endpoints enforce authentication and strict resource ownership (`user_id` / `tenant_id` scoping) on the server side.
   - **Database & API Contracts:** Verify that database migrations match `DATABASE.md`, indexes are applied, and API payloads match `API.md` response envelopes with strict input validation.
   - **Production Readiness & Observability:** Confirm that environment variables are documented in `.env.example`, health checks (`/api/health`) are live, error tracking/logging is active, and production build/typecheck commands pass cleanly with zero errors.

2. **Severity Classification & Blocker Triage:**
   Classify any discovered issues into:
   - **Blockers (P0):** Security vulnerabilities, broken core user journeys, data loss risks, or failing builds. *(Must be fixed before launch).*
   - **Warnings (P1):** Minor edge-case UI glitches, missing non-critical polish, or non-blocking performance optimizations. *(Can be deferred post-launch).*

3. **Determine the Launch Verdict:**
   Issue one of three official verdicts:
   - **`READY`**: All tests pass, zero blockers, architecture fully aligned with docs.
   - **`READY WITH WARNINGS`**: Core product is stable and secure; minor non-blocking items noted for fast-follow patches.
   - **`NOT READY`**: Critical blockers, security risks, or architectural violations present. Launch is blocked until resolved.

### STRICT RULES
- **Zero Hallucinated Verification:** Do not assume a feature works just because the file exists. Inspect the code, check test coverage, or run verification checks.
- **No Risky Auto-Patches:** Do not make sweeping architectural changes or rewrite complex logic during a final audit without explicit approval.

### OUTPUT & HANDOFF
Provide a formal **Final Production Sign-Off & Launch Report**:
1. **Official Launch Verdict:** (`READY`, `READY WITH WARNINGS`, or `NOT READY`)
2. **Audit Findings by Severity:** Bulleted list of Blockers, Warnings, and Passed checks across all modules.
3. **Tests & Build Verification:** Results of local production build, type check, linter, and test suite execution.
4. **Manual Pre-Launch Checklist:** Final human-verification items required before hitting the switch (e.g., configuring live DNS records, activating Stripe/Paystack live webhook endpoints, and setting production environment secrets in the hosting dashboard).