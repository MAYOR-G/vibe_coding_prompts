# 04. System Architecture Prompt (`architecture.md`)

> **How to use:** Paste this prompt in the **same chat session** right after finalizing `DESIGN.md` (or attach `PRD.md` and `DESIGN.md` if starting a fresh session). The AI will translate your product requirements and design system into a concrete, over-engineering-free technical blueprint for coding agents.

---

## Prompt

Now, act as a Principal Software Architect specializing in secure, maintainable, and pragmatic production applications.

### CONTEXT
Use our approved **`PRD.md`**, **`DESIGN.md`**, and the locked-in tech stack from our conversation above as your strict source of truth. *(If any critical infrastructure choice—such as hosting target, state management strategy, or database driver—is still ambiguous, ask me 1 to 3 quick clarifying questions first; otherwise, generate the full `ARCHITECTURE.md` immediately).*

### TASK
Create a comprehensive **`ARCHITECTURE.md`** that defines exactly how this product must be structured and built. Coding agents will rely on this document to make consistent file-placement, data-fetching, and dependency decisions without hallucinating unnecessary complexity.

Structure `ARCHITECTURE.md` with these exact sections:

1. **Architecture Overview & Core Philosophy**
   - High-level architectural pattern (e.g., Monolithic Full-Stack App, Decoupled SPA + API Backend, Static/Jamstack Site, or Event-Driven AI App).
   - Core engineering principles for this build (e.g., simplicity first, strict type safety, colocation, zero premature abstraction).

2. **Approved Tech Stack & Dependency Manifest**
   - Complete breakdown of every technology layer with clear reasoning for why it fits this specific MVP:
     - **Frontend:** Framework, language, styling engine, UI primitives, state management, and form/validation library.
     - **Backend / Runtime:** Server framework, runtime environment, and API paradigm.
     - **Database & Storage:** Primary database, ORM/query builder, caching layer (if any), and file/blob storage.
     - **Infrastructure & Hosting:** Deployment target, CI/CD pipeline, and DNS/edge layer.
   - **Approved Core Libraries vs. Banned Dependencies:** Explicitly list approved packages for common tasks (e.g., icons, dates, HTTP requests, validation) so coding agents do not install duplicate or conflicting libraries.

3. **Frontend & Backend System Architecture**
   - **Frontend Architecture:** Rendering strategy (SSR, SSG, CSR, or hybrid), routing mechanism, client vs. server component boundaries, global vs. local state management, and design token integration from `DESIGN.md`.
   - **Backend Architecture:** Service layer organization, business logic separation, background job/queue handling (if needed), and middleware pipeline. *(If the project is frontend-only, explicitly mark Backend as `N/A - Client/Static Architecture`).*

4. **End-to-End Data Flow & State Lifecycle**
   - Step-by-step trace of how data moves through the system:
     - **Read Flow:** User request -> Route/Controller -> Data fetching/caching -> UI rendering.
     - **Write/Mutation Flow:** Form submission -> Client validation -> API/Server Action -> Input sanitization -> Database transaction -> Cache invalidation & UI feedback.

5. **Exact Project Folder & File Structure**
   - Provide a complete, visual ASCII directory tree (`src/`, `app/`, `components/`, `lib/`, `services/`, `types/`, `tests/`, etc.).
   - Explicitly explain the responsibility of every top-level folder and naming conventions for files (e.g., `kebab-case` for files, `PascalCase` for components) so coding agents always place new files in the exact right location.

6. **Database Interaction & Data Access Layer**
   - Connection pooling strategy, ORM/client singleton pattern, migration workflow, transaction handling, and rules for where database queries are allowed to be executed (e.g., strictly inside `services/` or data-access modules, never directly inside UI components).

7. **Authentication, Authorization & Session Architecture**
   - Auth flow mechanics (JWT, HTTP-only sessions, OAuth, or magic links).
   - Where auth state is verified (middleware/edge vs. route handlers vs. data layer) and how role-based access control (RBAC) from `PRD.md` is enforced.

8. **API Architecture & Communication Contracts**
   - Internal and external API structure, route naming conventions, request validation strategy, standard response envelope format, pagination pattern, and rate-limiting rules.

9. **External Services & Third-Party Integrations**
   - Architectural pattern for wrapping third-party SDKs (payments, email, analytics, storage) behind clean adapter/service interfaces, including webhook verification, retry logic, and timeout handling.

10. **AI / LLM Architecture *(Include only if applicable; otherwise mark `N/A`)* **
    - Model provider routing, prompt management structure, streaming response handling, structured output validation, RAG/vector retrieval pipeline, token/cost guardrails, and fallback behavior when providers fail.

11. **Environment Configuration & Secrets Management**
    - Complete inventory of required environment variables (`.env.example` blueprint) categorized by **Public/Client-Safe** vs. **Server-Only Secrets**.
    - Rules for runtime environment validation on startup so the app fails fast if a required key is missing.

12. **Error Handling, Logging & Observability**
    - Standardized error hierarchy, global error boundaries, structured logging format (what to log vs. what PII to redact), and user-facing vs. developer-facing error reporting.

13. **Security Boundaries & Hardening Rules**
    - Input validation at trust boundaries, CORS policy, CSRF/XSS/SQLi protection, file upload validation, rate limiting, and content security headers.

14. **Testing, Deployment & Scalability Roadmap**
    - **Testing Strategy:** Pragmatic split between unit tests (core business logic), integration tests (API/DB), and E2E smoke tests (critical user journeys).
    - **Deployment Pipeline:** Local development -> Preview/Staging -> Production promotion and database migration execution.
    - **MVP vs. Future Scale:** Clearly separate what is built now for version 1.0 from what is deferred until traffic/scale demands it (e.g., no Redis, microservices, or Kubernetes in MVP).

### STRICT RULES
- **Pragmatism Over Hype:** Choose the simplest, most reliable architecture that fulfills `PRD.md`. Strictly forbid microservices, unnecessary message queues, or complex distributed patterns unless explicitly required.
- **Zero Implementation Code:** Do not write application code, SQL schemas, or full configuration scripts. Provide architectural rules, ASCII directory trees, and structural specifications only.
- **Deterministic Boundaries:** Leave zero ambiguity about where files live and how layers communicate.

### OUTPUT FORMAT
Return the complete `ARCHITECTURE.md` inside a single clean Markdown code block (` ```markdown ... ``` `) ready to save directly as `ARCHITECTURE.md`.