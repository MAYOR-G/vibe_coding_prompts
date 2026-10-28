# 14. External API & Third-Party Service Integration Prompt (`external_api_service_integration_prompts.md`)

> **How to use:** Paste this prompt into your coding agent whenever you are ready to integrate a third-party API or external service (e.g., Stripe/Paystack for payments, OpenAI/Gemini/OpenRouter for AI, Resend/SendGrid for email, S3/Cloudinary for storage). Replace `[INSERT SERVICE / API NAME + PURPOSE]` before running.

---

## Prompt

Act as a Principal Integration Engineer specializing in fault-tolerant, secure, and high-reliability production API integrations.

### CONTEXT
Your strict sources of truth are **`API.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, and **`agents_coding_rules.md`**.

Your assignment is to implement the following external integration:
**TARGET INTEGRATION:** `[INSERT SERVICE / API NAME + PURPOSE]`

### WORKFLOW (Execute strictly in this order):

1. **Reconnaissance & Contract Verification (Do This First):**
   - Inspect `API.md` and `ARCHITECTURE.md` to verify the approved SDK/HTTP client, folder placement, and exact data flow for this integration.
   - Check the codebase to see if an adapter, config file, or environment variable for this service already exists.

2. **Isolated Service Adapter Pattern:**
   - Encapsulate all third-party SDK calls or HTTP requests inside a dedicated server-side module/service file (e.g., `src/services/...` or `src/lib/integrations/...`).
   - **Never call third-party APIs directly inside UI components or scatter raw SDK calls across multiple route handlers.**
   - Validate required environment variables on initialization so the service fails fast with a clear developer message if an API key or base URL is missing.

3. **Request, Response & Schema Validation:**
   - Implement the exact operations required by `API.md`.
   - Validate all outbound arguments and strictly parse/validate inbound third-party responses (using Zod/Pydantic or strict TypeScript types) so unexpected upstream payload changes never crash our application silently.

4. **Resilience: Timeouts, Retries & Rate Limits:**
   - **Explicit Timeouts:** Every external request must have a strict timeout configured (never leave external requests hanging indefinitely).
   - **Safe Retries & Idempotency:** Implement exponential backoff retries **only** for transient network errors (`502`, `503`, `504`, `429`) and idempotent operations. For state-mutating calls (like charging a card or sending an email), pass an explicit `Idempotency-Key` header if supported to prevent duplicate charges/actions.
   - **Rate-Limit Handling:** Respect upstream `429 Too Many Requests` and `Retry-After` headers gracefully, surfacing a clean, user-friendly fallback message via our standard error envelope.

5. **Webhooks & Cryptographic Verification *(If applicable to this service)*:**
   - Implement the dedicated webhook endpoint defined in `API.md`.
   - **Verify Signatures First:** Cryptographically verify the provider's webhook signature using the raw request body and webhook secret *before* parsing or trusting any data. Reject invalid signatures immediately with `400`/`401`.
   - **Replay & Duplicate Protection:** Log processed webhook `event_id`s in the database (per `DATABASE.md`) and wrap state updates in a database transaction so duplicate webhook deliveries are safely ignored.

6. **Privacy-Safe Logging & Observability:**
   - Log integration failures with structured context (service name, operation, HTTP status, request ID, latency) while strictly redacting API keys, bearer tokens, payment details, and user PII.

7. **Deterministic Automated Testing:**
   - Write unit/integration tests that **mock** the external provider's responses (never hit live paid/rate-limited external APIs in automated test suites) covering:
     - Successful response parsing (`200 OK`).
     - Upstream error handling (`400`, `401`, `429`, `500`, and network timeout).
     - Webhook signature verification (valid signature vs. tampered signature) and duplicate event idempotency.

### STRICT RULES
- **Server-Only Secrets:** Never import server integration modules or expose API keys in client-side code (`NEXT_PUBLIC_` / `VITE_` variables are strictly forbidden for secret keys).
- **Zero Scope Creep:** Implement only the specific operations defined in `API.md`. Do not build extra wrapper methods for provider features our MVP does not use.
- **No Unapproved Dependencies:** Use the HTTP client or official SDK approved in `ARCHITECTURE.md`. Ask before installing any new package.

### OUTPUT & HANDOFF
Once the integration is implemented and verified:
1. Make an atomic Git commit (`feat: integrate [service-name] service and error handling`).
2. Provide a concise summary including:
   - Exact environment variables added to `.env.example`.
   - Files created/modified and the adapter functions exposed.
   - How timeouts, retries, and webhooks (if any) are handled.
   - Results of the mocked success and failure test cases.
   