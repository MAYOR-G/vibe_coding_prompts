# 05. Database Schema & Data Modeling Prompt (`database.md`)

> **How to use:** Paste this prompt in the **same chat session** right after finalizing `ARCHITECTURE.md` (or attach `PRD.md` and `ARCHITECTURE.md` if starting a new session). The AI will design a complete, deterministic database blueprint that coding agents can later translate directly into SQL migrations or ORM schemas without guessing column names or relationships.

---

## Prompt

Now, act as a Principal Database Architect and Senior Backend Engineer specializing in reliable, high-performance production data models.

### CONTEXT
Use our approved **`PRD.md`** and **`ARCHITECTURE.md`** as your strict source of truth. Every table, field, relationship, and access policy must directly support the user journeys, roles, and architecture we have locked in. *(Note: If this project is a static website or stateless client app with no database, explicitly document client-side persistence/local storage only and mark server database sections as `N/A`).*

### TASK
Create a complete, production-ready **`DATABASE.md`** that serves as the single source of truth for the application's data layer.

Structure `DATABASE.md` with these exact sections:

1. **Database Technology & Configuration Overview**
   - Chosen database engine (e.g., PostgreSQL, SQLite, MySQL, MongoDB, Supabase, plus any Vector DB if applicable) and ORM/query layer from `ARCHITECTURE.md`.
   - Naming conventions enforced across the schema (e.g., `snake_case` plural table names, `snake_case` columns, standardized primary key strategy such as `UUID v4/v7` vs. auto-incrementing integers).
   - Standard audit columns applied across tables (`created_at`, `updated_at`, and explicit rules on when to use hard deletes vs. `deleted_at` soft deletes).

2. **Entity-Relationship (ER) Diagram & Cardinality**
   - A clean **Mermaid.js `erDiagram`** block illustrating all tables and their relationships (`1:1`, `1:N`, `M:N`).
   - A concise bulleted summary explaining how the core entities connect and the exact join tables used for many-to-many relationships.

3. **Enums & Standardized Value Sets**
   - Explicit inventory of all Enum types or constrained status strings (e.g., `user_role`, `subscription_status`, `order_state`) with their exact casing and allowed values so frontend, backend, and database never drift out of sync.

4. **Complete Table & Column Specifications**
   - For every table in the MVP, provide a dedicated Markdown table specifying:
     - **Column Name**
     - **Data Type** (exact DB type, e.g., `UUID`, `VARCHAR(255)`, `TEXT`, `TIMESTAMPTZ`, `NUMERIC(10,2)`, `JSONB`, `BOOLEAN`)
     - **Nullable?** (`NOT NULL` vs. `NULL`)
     - **Default Value** (e.g., `gen_random_uuid()`, `now()`, `false`)
     - **Constraints** (`PK`, `FK -> table.column`, `UNIQUE`, `CHECK`)
     - **Description / Purpose**

5. **Foreign Keys & Referential Integrity Rules**
   - Explicit mapping of every foreign key relationship, including exact **`ON DELETE`** (`CASCADE`, `SET NULL`, `RESTRICT`) and **`ON UPDATE`** behaviors so orphaned records or accidental data loss cannot occur.

6. **Data Integrity Constraints & DB-Level Validation**
   - Composite unique constraints (e.g., `UNIQUE(organization_id, slug)`).
   - Database-level `CHECK` constraints (e.g., `price >= 0`, `end_date > start_date`) that protect data integrity even if application validation is bypassed.

7. **Query-Driven Indexing Strategy**
   - List every non-primary index (`B-Tree`, `GIN`, `Unique`, or `Composite`) and pair each index with the **exact real-world query or filter pattern from `PRD.md`** it optimizes (e.g., fetching a user's dashboard items ordered by `created_at DESC`).
   - Explicitly avoid speculative or redundant indexes that slow down writes.

8. **Data Ownership, Multi-Tenancy & Authorization Model**
   - How record ownership is established (e.g., every resource tied to `user_id` or `workspace_id`).
   - **Row-Level Security (RLS) & Access Policies:**
     - If using a platform with RLS (like Supabase/PostgreSQL), define the exact `SELECT`, `INSERT`, `UPDATE`, and `DELETE` policy logic per table for each role (`anon`, `authenticated`, `admin`, `service_role`).
     - If enforcing authorization at the application/ORM service layer, define the mandatory query scoping rules (e.g., every query must filter by the authenticated session's `user_id`).

9. **Sensitive Data, PII & Encryption Handling**
   - Inventory of sensitive fields (passwords, tokens, API keys, PII, financial/health records).
   - Exact storage rules: hashing algorithms, encryption-at-rest requirements, column redaction in logs, and fields that must **never** be returned in standard API responses.

10. **AI / Vector & Unstructured Storage *(Include only if applicable; otherwise mark `N/A`)* **
    - Vector embedding dimensions, similarity metric (`cosine`, `l2`, `inner_product`), metadata filtering schema, and how vector chunks reference source records in the relational database.
    - Object/blob storage bucket structure for files/media and how file paths/URLs are referenced in database columns.

11. **Migrations, Seeding & Lifecycle Strategy**
    - How schema changes are generated, reviewed, and applied across local, staging, and production environments.
    - **Seed Data Blueprint:** Exact initial records required to run and test the app locally (e.g., default admin user, sample categories, mock records for each core state).
    - Zero-downtime migration rules (handling column renames, non-null additions, and rollbacks safely).

12. **Backup, Retention & Recovery Considerations**
    - Automated backup frequency, Point-in-Time Recovery (PITR) needs, data retention/purging policies, and account deletion cascade behavior (GDPR/privacy compliance).

### STRICT RULES
- **Lean & Normalized MVP Schema:** Design only the tables and columns required to power the approved `PRD.md`. Do not invent speculative tables for unrequested future features.
- **No Application Code:** Do not write backend route handlers, ORM model classes, or raw SQL migration scripts yet. Use structured Markdown tables, constraint definitions, and a Mermaid `erDiagram` so any coding agent can generate the exact ORM/SQL implementation deterministically during setup.
- **Zero Ambiguity:** Never write vague descriptions like *"stores user metadata."* List the exact columns and types.

### OUTPUT FORMAT
Return the complete `DATABASE.md` inside a single clean Markdown code block (` ```markdown ... ``` `) ready to save directly as `DATABASE.md`.