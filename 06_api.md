# 06. API & Integration Contracts Prompt (`api.md`)

> **How to use:** Paste this prompt in the **same chat session** right after finalizing `DATABASE.md` (or attach `PRD.md`, `ARCHITECTURE.md`, and `DATABASE.md` if starting a new session). The AI will define the exact internal and external API contracts so frontend and backend coding agents never mismatch routes, payload keys, or status codes.

---

## Prompt

Now, act as a Principal Backend Architect and API Integration Engineer specializing in secure, fault-tolerant production APIs.

### CONTEXT
Use our approved **`PRD.md`**, **`ARCHITECTURE.md`**, and **`DATABASE.md`** as your strict source of truth. Every route, payload field, and permission check must align 1:1 with the features in `PRD.md`, the architectural patterns in `ARCHITECTURE.md`, and the exact column/enum names in `DATABASE.md`. *(Note: If the project uses Next.js Server Actions, tRPC, or BaaS SDKs alongside REST endpoints, document both traditional HTTP routes and RPC/Server Action contracts clearly).*

### TASK
Create a comprehensive **`API.md`** that serves as the single source of truth for all internal client-to-server communication and external third-party service integrations.

Structure `API.md` with these exact sections:

1. **API Design Philosophy & Global Conventions**
   - **Communication Paradigm:** REST HTTP endpoints, Server Actions / RPC, GraphQL, WebSockets, or SSE (Server-Sent Events for AI/real-time streaming).
   - **Base URL & Versioning:** Base path structure (e.g., `/api/v1/...`), trailing slash rules, and resource naming conventions (`kebab-case` plural nouns).
   - **Data Formatting Standards:** JSON property casing (`camelCase` vs. `snake_case` mapping to `DATABASE.md`), ISO-8601 UTC timestamp format, currency/decimal representation, and `null` vs. omitted field rules.
   - **Standard Headers:** Required headers for requests and responses (`Content-Type`, `Authorization`, `X-Request-ID`, `Idempotency-Key` for critical mutations).

2. **Standard Response Envelopes & Error Format**
   - **Success Envelope:** Exact JSON structure for single-item responses and list/paginated responses.
   - **Error Envelope:** Standardized error schema (`error.code`, `error.message`, `error.fieldErrors`, `requestId`) so the frontend can parse and display inline or toast errors deterministically.
   - **HTTP Status Code Matrix:** Exact mapping of when to return `200`, `201`, `204`, `400`, `401`, `403`, `404`, `409`, `422`, `429`, and `500`.

3. **Authentication, Authorization & RBAC Enforcement**
   - How credentials/tokens are passed (e.g., `HttpOnly` secure cookies, `Bearer` JWT in headers, API keys for machine-to-machine).
   - Server-side authorization matrix: How every endpoint verifies both **identity** (who is calling) and **resource ownership** (does this user/tenant own the target `id` in `DATABASE.md`?).

4. **Complete Internal Endpoint / Action Inventory**
   - Group endpoints logically by domain/module (e.g., `Auth`, `Users`, `Core Resources`, `Billing`, `Admin`).
   - For **every single endpoint or Server Action**, specify:
     - **Method & Path:** (e.g., `POST /api/v1/projects` or `Action: createProject`)
     - **Purpose:** Which user story in `PRD.md` this powers.
     - **Access Level:** `Public`, `Authenticated User`, `Resource Owner Only`, or `Admin Only`.
     - **Request Parameters & Body:** Path params, query params, and request payload fields with types and validation constraints (required/optional, min/max, enum values).
     - **Response Payload:** Exact fields returned on success (explicitly excluding sensitive `DATABASE.md` columns like password hashes or internal tokens).
     - **Specific Failure Cases:** Expected domain errors (e.g., `409 EMAIL_ALREADY_EXISTS`, `403 INSUFFICIENT_CREDITS`).

5. **Pagination, Filtering, Sorting & Search Conventions**
   - Standardized query parameters for list endpoints (e.g., cursor-based `cursor` & `limit` vs. offset-based `page` & `pageSize`, `sortBy`, `sortOrder`, `search`).
   - Default and maximum page size limits to prevent accidental full-table dumps.

6. **Streaming, Real-Time & Async Operations *(Include if applicable; otherwise mark `N/A`)* **
   - **AI / LLM Streaming:** Protocol (`SSE` / `ReadableStream`), chunk event structure (`text-delta`, `tool-call`, `done`, `error`), and client abort/cancellation handling.
   - **File Uploads:** Presigned URL flow vs. multipart form-data, allowed MIME types, and maximum file size limits.

7. **External APIs & Third-Party Service Integrations**
   - Complete inventory of external services (e.g., Stripe/Paystack, OpenAI/Gemini/OpenRouter, Resend/SendGrid, S3/Cloudinary, OAuth providers).
   - For each external integration, define:
     - **Service Purpose & Direction:** Outbound API calls vs. Inbound Webhooks.
     - **Service Adapter Boundary:** Which internal backend module wraps this integration (never call third-party APIs directly from UI components).
     - **Timeouts, Retries & Circuit Breaking:** Exact timeout thresholds, exponential backoff rules, and fallback behavior if the third-party service is down.

8. **Webhooks & Inbound Event Verification**
   - Dedicated webhook endpoints (e.g., `POST /api/webhooks/stripe`).
   - **Cryptographic Signature Verification:** Exact header verification steps required before processing any payload.
   - **Idempotency & Replay Protection:** How webhook event IDs are tracked in `DATABASE.md` so duplicate deliveries never trigger double-charging or duplicate state updates.

9. **Rate Limiting, Abuse Prevention & Quotas**
   - Tiered rate-limiting rules by endpoint category:
     - **Strict:** Auth/Login/Password Reset & AI generation endpoints.
     - **Standard:** Authenticated CRUD mutations.
     - **Relaxed:** Public read endpoints.
   - Rate-limit response headers (`Retry-After`, `X-RateLimit-Remaining`).

10. **Data Privacy, Sanitization & Secret Boundaries**
    - **Server-Only Secret Enforcement:** Explicit rule that third-party API keys and service-role tokens never cross the client serialization boundary.
    - **Input Sanitization & Output Redaction:** Stripping HTML/XSS payloads on input and redacting PII/secrets from error responses and server logs.

11. **Required API Environment Variables**
    - Exact list of environment variable names, base URLs, webhook secrets, and timeout configurations required for all internal and external APIs.

### STRICT RULES
- **Lean & Scoped to MVP:** Define only the endpoints and integrations required by `PRD.md`. Do not invent speculative CRUD endpoints that no screen in `DESIGN.md` uses.
- **Exact Schema Alignment:** Every field name and enum referenced in request/response payloads must match `DATABASE.md` and `PRD.md` without drift.
- **Zero Implementation Code:** Do not write Express/FastAPI/Next.js route handler code or SDK boilerplate. Write clean specification tables and contract schemas only.

### OUTPUT FORMAT
Return the complete `API.md` inside a single clean Markdown code block (` ```markdown ... ``` `) ready to save directly as `API.md`.