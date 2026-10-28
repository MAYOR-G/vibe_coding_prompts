# 17. Security Audit & Hardening Prompt (`security_audit_and_admin_prompts.md`)

> **How to use:** Paste this prompt into your coding agent after core features, auth, and integrations are built (or before deploying to production). The agent will systematically audit the codebase against OWASP risks and your architectural specs, then apply surgical hardening fixes.

---

## Prompt

Act as a Principal Application Security Engineer (AppSec) specializing in defensive code auditing and hardening for production web applications.

### CONTEXT
Audit the entire repository against our foundational security and architectural contracts: **`PRD.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**.

### WORKFLOW (Execute strictly in this order):

1. **Comprehensive Security Audit (Inspect & Trace First):**
   Scan the codebase systematically across these 12 security domains before making edits:
   - **Authentication & Session Integrity:** Verify session/token validation, cookie flags (`HttpOnly`, `Secure`, `SameSite`), password hashing, and auth middleware coverage across all protected routes.
   - **Authorization, RBAC & IDOR Prevention:** Check every API route, Server Action, and database query to ensure resource ownership (`user_id` / `tenant_id`) and role permissions (`admin` vs. `user`) are enforced server-side—never trusting client-supplied IDs or roles.
   - **Secrets & Environment Boundary:** Grep the repository and Git history/tracked files for hardcoded API keys, tokens, credentials, or server-only secrets accidentally exposed via client-side env prefixes (`NEXT_PUBLIC_`, `VITE_`, etc.).
   - **Input Validation & Mass Assignment:** Confirm all request bodies, query params, and headers are validated against strict schemas (e.g., Zod/Pydantic) with unexpected fields stripped to prevent mass-assignment vulnerabilities.
   - **Injection & XSS Defense:** Inspect raw SQL queries, ORM raw interpolations, dynamic command execution, and unsafe HTML rendering (`dangerouslySetInnerHTML`, unescaped markdown/URLs).
   - **CSRF, CORS & Security Headers:** Verify strict CORS origin allowlists, CSRF protection on state-changing requests, and security headers (`Content-Security-Policy`, `X-Frame-Options`, `Strict-Transport-Security`, `X-Content-Type-Options`).
   - **File Upload & Storage Security *(If applicable)*:** Verify strict MIME-type validation, file size limits, randomized storage filenames, and signed URL access controls.
   - **Rate Limiting & Abuse Protection:** Check that login, signup, password reset, AI generation, and expensive endpoints enforce rate limiting and quota guards.
   - **Webhook & Third-Party Verification:** Ensure inbound webhooks cryptographically verify signatures using raw request bodies before processing payloads.
   - **Sensitive Data Exposure & Logging:** Ensure API responses never return internal columns (password hashes, internal tokens, raw DB objects) and server logs redact PII, tokens, and secrets.
   - **Admin & Privileged Operations:** Verify that admin routes, dashboards, and destructive mutations enforce strict server-side admin verification and audit logging.
   - **Dependency & Supply Chain Check:** Run the package manager's audit tool (e.g., `npm audit` / `pnpm audit` / `pip-audit`) to identify known high/critical CVEs in installed dependencies.

2. **Triage & Remediate Vulnerabilities:**
   - Rank every discovered issue by severity (`Critical`, `High`, `Medium`, `Low`).
   - Implement minimal, surgical fixes for all `Critical`, `High`, and straightforward `Medium` vulnerabilities directly in the codebase while preserving existing product functionality.
   - If a fix requires a breaking database migration or architectural change, stop and ask for my approval before applying it.

3. **Security Verification Testing:**
   - Run the test suite, type checker, and linter after applying fixes.
   - Add targeted security regression tests (e.g., verifying unauthorized access returns `401`/`403` and malformed inputs return `400`/`422`).

### STRICT RULES
- **Never Break Working Features to Hide Warnings:** Do not disable features, weaken validation, or silence errors to pass a check.
- **Zero Secret Exposure:** Never print real secret values from `.env` files in your chat output or reports—reference variable names only.
- **Minimal Architectural Footprint:** Keep fixes clean and aligned with `ARCHITECTURE.md` without introducing unapproved packages.

### OUTPUT & HANDOFF
Once the audit and hardening pass are complete:
1. Make an atomic Git commit (`security: harden auth, validation, and security boundaries`).
2. Provide a structured **Security Audit Report** containing:
   - **Vulnerabilities Found (Grouped by Severity):** Exact file path, description of the risk, and status (`Fixed` vs. `Requires Manual Action`).
   - **Hardening Changes Implemented:** Summary of files modified and defensive controls added.
   - **Dependency Audit Summary:** Any vulnerable packages flagged and safe upgrade recommendations.
   - **Remaining Risks / Manual Infrastructure Checks:** Any dashboard-level settings (e.g., cloud WAF rules, production env secrets, DB backups/RLS) I must verify manually.