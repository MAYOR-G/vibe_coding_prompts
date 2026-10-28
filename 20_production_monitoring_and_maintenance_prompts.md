# 20. Production Monitoring, Observability & Maintenance Prompt (`production_monitoring_and_maintenance_prompts.md`)

> **How to use:** Paste this prompt into your coding agent once your application is deployment-ready or live in production. It sets up structured logging, error tracking, health checks, cost/quota guards, and operational runbooks without bloated enterprise overhead.

---

## Prompt

Act as a Principal Site Reliability Engineer (SRE) and Production Operations Specialist focused on pragmatic, high-signal observability and system resilience.

### CONTEXT
Review **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, **`PRD.md`**, and **`agents_coding_rules.md`**, along with the current production setup in the codebase.

**MONITORING & TOOLING PREFERENCE:** `[USE ARCHITECTURE.MD DEFAULTS / OR SPECIFY E.G., SENTRY, POSTHOG, PINO/STRUCTURED LOGS, UPTIME MONITOR]`

### WORKFLOW (Execute strictly in this order):

1. **Observability & Logging Audit (Do This First):**
   - Inspect existing logging statements, error boundaries, and health-check routes across the codebase.
   - Replace scattered, unstructured `console.log` calls in server/API code with a clean, structured logger (JSON format in production, readable format in development) that attaches contextual metadata (`timestamp`, `level`, `requestId`, `route`, `durationMs`, `userId` if authenticated).

2. **Application Error & Exception Tracking:**
   - Ensure unhandled server exceptions, API route crashes, and client-side React/UI error boundaries are captured consistently.
   - Enrich error reports with actionable debugging context (HTTP method, route path, status code, error code) while stripping all sensitive payloads.

3. **API, Database & External Integration Telemetry:**
   - **API Latency & Failure Tracking:** Log slow requests (exceeding target thresholds in `PRD.md`) and all `5xx` server failures or spikes in `401`/`403`/`429` responses.
   - **Database Health:** Monitor connection pool exhaustion, failed transactions, and slow queries.
   - **Third-Party & AI Integration Monitoring:** Track external service failures, webhook verification failures, timeout rates, and (if applicable) AI/LLM token usage, latency, and provider fallback events so cloud/API costs cannot silently spike.

4. **Health Check & Uptime Readiness Endpoints:**
   - Verify or implement a production health check endpoint (e.g., `GET /api/health`) suitable for external uptime monitors and container orchestrators:
     - Returns `200 OK` with minimal safe status metadata (`status: "ok"`, `timestamp`, `uptime`) when the app and database are reachable.
     - Returns `503 Service Unavailable` if critical dependencies (like the primary database) are unreachable.

5. **Critical Business Event & Funnel Tracking:**
   - Instrument lightweight, privacy-safe tracking for the core MVP success events defined in `PRD.md` (e.g., user signup completed, core workflow executed, payment/webhook succeeded or failed).

6. **High-Signal Alerting & Backup Verification Blueprint:**
   - Define actionable alert thresholds (avoiding noisy alerts):
     - **Immediate Pager/Critical Alerts:** App downtime (`503` health check), spike in `5xx` error rate, database connection failure, or payment/webhook processing failure.
     - **Warning Alerts:** Elevated API latency, rate-limit spikes, or approaching third-party API quota limits.
   - Document the operational runbook for database backup verification, Point-in-Time Recovery (PITR) testing, and safe rollback procedures.

### STRICT RULES
- **Zero PII or Secret Leakage in Logs:** Never log plaintext passwords, session cookies, `Authorization` headers, API keys, payment card details, or sensitive user PII. Implement an explicit redaction list in the logger.
- **Right-Sized for MVP Scale:** Keep the monitoring setup lean, cost-effective, and aligned with `ARCHITECTURE.md`. Do not introduce heavy enterprise infrastructure (like self-hosted Prometheus/Grafana/ELK clusters) unless explicitly requested.
- **Non-Blocking Telemetry:** Logging and analytics calls must never block or crash the user's primary request flow if a telemetry service is slow or unreachable.

### OUTPUT & HANDOFF
Once the observability and maintenance setup is implemented and verified:
1. Make an atomic Git commit (`chore: configure structured logging, health checks, and production observability`).
2. Provide a structured **SRE Observability & Maintenance Report**:
   - **Telemetry Implemented:** Summary of structured logging, error boundaries, and health-check endpoints configured.
   - **Redaction & Privacy Safeguards:** Confirmation of what fields are automatically scrubbed from logs.
   - **Recommended Alert Rules:** Exact conditions and thresholds I should toggle on in my hosting/monitoring dashboard.
   - **Backup, Rollback & Maintenance Runbook:** Quick-reference checklist for verifying backups, rolling back a bad deploy, and rotating secrets safely.