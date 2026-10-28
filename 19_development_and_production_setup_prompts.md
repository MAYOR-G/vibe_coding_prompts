# 19. Deployment & Production Setup Prompt (`development_and_production_setup_prompts.md`)

> **How to use:** Paste this prompt into your coding agent when your features, QA, and security checks are complete and you are ready to prepare and execute the production deployment (e.g., Vercel, Railway, Render, Fly.io, AWS, or Docker/VPS).

---

## Prompt

Act as a Principal DevOps, Cloud Infrastructure, and Release Engineer specializing in zero-downtime, secure production deployments.

### CONTEXT
Follow **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`** as your strict source of truth.

**TARGET DEPLOYMENT PLATFORM:** `[INSERT HOSTING PROVIDER FROM ARCHITECTURE.MD OR SPECIFY HERE]`

### WORKFLOW (Execute strictly in this order):

1. **Local Production Build & Pre-Flight Verification (Do This First):**
   - Run the full production build command locally (`npm run build`, `pnpm build`, or Docker build) alongside strict type checking and linting.
   - Fix any build-time errors, SSR/static generation issues, missing dependency declarations (`dependencies` vs. `devDependencies`), or environment validation crashes before touching deployment configs.

2. **Environment Variable & Secret Separation:**
   - Audit and update `.env.example` so every single variable required in production is documented and categorized into:
     - **Public / Build-Time Variables**
     - **Server-Only Runtime Secrets**
   - Ensure strict separation between Development, Staging/Preview, and Production credentials (e.g., live vs. test API keys, production database URLs, production webhook secrets).
   - Ensure startup validation fails fast with a clear log message if any required production environment variable is missing.

3. **Deployment Configuration & Infrastructure Files:**
   - Create or verify the required platform configuration files specified in `ARCHITECTURE.md` (e.g., `Dockerfile`, `.dockerignore`, `docker-compose.prod.yml`, `vercel.json`, `railway.toml`, `fly.toml`, or GitHub Actions CI/CD workflows).
   - Configure exact build commands, start commands, Node/Python runtime versions, and server port bindings.

4. **Production Database & Safe Migration Strategy:**
   - Verify production database connection pooling settings (e.g., PgBouncer / serverless pooler URL vs. direct migration URL).
   - Define the safe, non-destructive production migration command (e.g., `prisma migrate deploy`, `alembic upgrade head`, or Drizzle migration runner).
   - **Strict Rule:** Never run `migrate dev`, `db push --force-reset`, or destructive seed scripts against staging or production databases.

5. **Networking, CORS, Domains & Security Headers:**
   - Replace any hardcoded `http://localhost:...` URLs in code or configs with dynamic environment-driven base URLs.
   - Lock down production CORS origins, cookie security flags (`Secure`, `HttpOnly`, `SameSite`), canonical URLs, OAuth redirect URIs, and webhook endpoint URLs.

6. **Health Checks & Graceful Shutdown:**
   - Implement or verify a lightweight health-check endpoint (e.g., `GET /api/health`) that verifies application uptime and database connectivity without exposing sensitive diagnostics.
   - Configure graceful shutdown signal handling (`SIGTERM` / `SIGINT`) where applicable for containerized/server runtimes so in-flight requests and database connections close cleanly.

### STRICT RULES
- **Zero Production Data Loss:** Never execute destructive database commands, drop tables, or overwrite production state. Always ask for my explicit confirmation before running live production migrations.
- **No Committed Secrets:** Double-check `git status` and `.gitignore` to ensure `.env`, `.env.production`, private keys, or cloud CLI credentials are never staged or committed.
- **Verify Before Declaring Done:** Do not mark deployment setup complete until the local production build succeeds with zero errors.

### OUTPUT & HANDOFF
Once the production setup is verified, provide a structured **Deployment Readiness & Release Report**:
1. **Production Build Verification:** Results of the local production build, typecheck, and health-check endpoint test.
2. **Infrastructure Files Created/Updated:** Summary of deployment configs, scripts, and runtime settings.
3. **Production Environment Checklist:** Exact table of environment variables I need to paste into my hosting provider's dashboard.
4. **Database Migration Plan:** Exact command to run for production schema migrations and confirmation that it is non-destructive.
5. **Manual Go-Live Steps:** Step-by-step checklist for me to finalize DNS/HTTPS, OAuth callback URLs, and live webhook endpoints.