# 02. Product Requirements Document Prompt (`prd_creation.md`)

> **How to use:** Paste this prompt in the **exact same chat session** right after completing the `idea.md` discovery conversation. The AI will automatically use the context and Locked-In Project Brief you just discussed—no need to re-explain your project idea.

---

## Prompt

Now, act as a Senior Product Manager and Principal Software Architect experienced in taking products from raw concept to production. 

### CONTEXT
Use the **Locked-In Project Brief** and all product, feature, and tech stack decisions from our conversation above as your foundational context. *(If any critical detail is still missing, briefly ask me before generating the full document; otherwise, proceed directly).*

### TASK
Generate a comprehensive, production-grade **`PRD.md`** that will serve as the single source of truth for AI coding agents and developers throughout the build. 

Structure the `PRD.md` with the following exact sections:

1. **Project Overview & Executive Summary**
   - **Problem Statement:** The specific pain point or gap being addressed.
   - **Value Proposition:** What makes this solution effective and distinct.
   - **Target Users & Personas:** Primary user types and their core goals.
   - **Approved Tech Stack:** Summary of the locked-in frontend, backend, database, auth, and third-party tools.

2. **Core User Journeys & User Stories**
   - Step-by-step end-to-end flows from the user's entry point to core value completion.
   - User stories formatted as: *"As a [role], I want to [action], so that [outcome]"* with clear **Acceptance Criteria** for each.

3. **Features & Functional Requirements**
   - Grouped logically by module or feature area.
   - Priority tags for every requirement (`P0 - Must Have for MVP`, `P1 - Important`, `P2 - Future/Nice-to-Have`).
   - Explicit business rules, validation logic, and edge-case behaviors.

4. **Pages, Screens & Navigation Flow**
   - Complete inventory of every page, view, modal, or screen in the application.
   - URL routes (e.g., `/`, `/dashboard`, `/settings`), core components on each screen, and how pages link together.

5. **Roles, Authentication & Permissions**
   - User roles (e.g., Guest, Authenticated User, Admin) and an explicit access-control matrix defining what each role can view, create, update, or delete. *(If the project is a simple static website with no auth, explicitly state `N/A - Public Access Only`).*

6. **Data Requirements & Core Entities**
   - High-level overview of the core data models/entities, key attributes, and relationships required to power the features (avoiding raw SQL code, focusing on conceptual data requirements and state persistence).

7. **External Integrations & APIs**
   - Third-party services, webhooks, AI models, payment gateways, or external APIs required, including what data flows in and out of each.

8. **UI/UX States (Loading, Empty & Error States)**
   - **Loading States:** Skeleton loaders, spinners, or optimistic UI behavior per key view.
   - **Empty States:** What a first-time user sees when no data exists yet (including call-to-action prompts).
   - **Error & Fallback States:** How form validation errors, network failures, 404s, and unauthorized access are communicated to the user.

9. **Non-Functional Requirements**
   - Performance targets, responsiveness (mobile/desktop breakpoints), accessibility (a11y), SEO needs, security constraints, and browser/device compatibility.

10. **MVP Scope, Out-of-Scope & Success Criteria**
    - **Strictly In-Scope (MVP):** The exact boundary of version 1.0.
    - **Strictly Out-of-Scope (Non-Goals):** Features explicitly excluded so coding agents do not build them prematurely.
    - **Definition of Done & Success Metrics:** Testable criteria confirming the MVP works as intended.

11. **Assumptions & Open Decisions Requiring Approval**
    - Clearly labeled assumptions made while drafting this PRD.
    - Specific architectural or product decisions flagged for my final sign-off before we move to `design.md`.

### STRICT RULES
- **Zero Code:** Do not write implementation code, SQL scripts, or component boilerplate.
- **No Feature Creep:** Do not invent unrequested features or over-engineer the scope. Scale the depth proportionally to the project (keep a simple website lean; make a full-stack app thorough).
- **Agent-Ready Formatting:** Use crisp Markdown tables, bulleted checklists, and unambiguous technical language so downstream coding agents can parse rules deterministically.

### OUTPUT FORMAT
Output the entire `PRD.md` inside a single, clean Markdown code block (` ```markdown ... ``` `) so I can save it directly as `PRD.md`, followed by a brief prompt asking me to confirm any flagged decisions before we move to `design.md`.