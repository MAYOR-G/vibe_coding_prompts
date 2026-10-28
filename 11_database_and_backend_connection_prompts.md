# 11. Database & Backend Connection Prompt (`database_and_backend_connection_prompts.md`)

> **How to use:** Use this prompt during Phase 1 & 2 of your `IMPLEMENTATION_PLAN.md`. Paste it into your coding agent when you need it to initialize the database connection, generate migrations, set up the ORM, and scaffold the core backend services without hallucinating random tables.

---

## Prompt

Act as a Principal Backend Engineer and Database Specialist experienced in building secure, high-performance production systems.

### CONTEXT
Your absolute sources of truth for this task are **`DATABASE.md`**, **`ARCHITECTURE.md`**, **`API.md`**, and **`agents_coding_rules.md`**. You are strictly forbidden from inventing tables, columns, or endpoints that are not explicitly documented in these files.

### TASK
Your task is to securely connect the application to the database and implement the core data access layer.

Execute the following workflow thoroughly. Use your terminal capabilities to run migration scripts or install necessary database drivers if they are missing (and approved in `ARCHITECTURE.md`).

1. **Secure Configuration & ORM Setup:**
   - Configure the database connection using the approved ORM or query builder.
   - Ensure connection strings and credentials are read strictly from environment variables (e.g., `process.env.DATABASE_URL`). **Never hardcode credentials.**
   - Implement connection pooling logic if appropriate for our architectural framework (e.g., handling serverless database connections).

2. **Schema Translation & Migrations:**
   - Translate the exact tables, columns, types, and constraints from `DATABASE.md` into the formal schema syntax (e.g., Prisma schema, Drizzle schema, SQLAlchemy models, or raw SQL).
   - Generate the initial migration files.
   - Apply the migration to the local development database.

3. **Data Access Layer & Models:**
   - Create the core repository/service functions for CRUD operations.
   - Enforce database-level constraints, relationships, and cascading deletes exactly as specified.
   - Apply the specific query-driven indexes mentioned in `DATABASE.md`.

4. **Seed Data Implementation:**
   - Create a deterministic seed script that populates the database with the exact development data specified in `DATABASE.md` (e.g., default admin user, test categories).

5. **Validation, Authorization & Error Handling:**
   - Implement the data validation schemas (e.g., Zod, Pydantic) matching the database columns.
   - Set up the server-side authorization checks (or Row-Level Security policies) to ensure users can only access their own tenant/data.
   - Implement transaction blocks for operations that mutate multiple tables to prevent orphaned records.

### STRICT RULES
- **No Rogue Schema Changes:** Do not modify the database structure, invent new columns (like adding `is_active` if it's not in `DATABASE.md`), or change relationships without explicit justification and my approval.
- **Fail-Safe Execution:** If a migration fails, stop and report the error immediately. Do not attempt to force/drop tables without asking.
- **Enforce Server-Side Auth:** Never trust client-side data. All data access must be validated and authorized at the server layer.

### OUTPUT & HANDOFF
Once the backend connection and schema are fully implemented:
1. Make an atomic Git commit (e.g., `feat: setup database connection and initial schema`).
2. Briefly report the tables created, migrations run, and verify that the seed script executed successfully.
3. Tell me what the next logical task from the implementation plan should be.