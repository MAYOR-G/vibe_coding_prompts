# 13. MCP (Model Context Protocol) Connection Prompt (`mcp_connection_prompts.md`)

> **How to use:** Paste this prompt into your coding agent (Cursor, Windsurf, Claude Code, etc.) whenever you want to connect an MCP server (such as Supabase, Postgres, GitHub, Stripe, Context7, Playwright, or a custom MCP server) to give your coding environment or application direct, controlled tool access. Replace `[INSERT MCP SERVER / PURPOSE]` before running.

---

## Prompt

Act as a Principal AI Systems Engineer and MCP (Model Context Protocol) Integration Specialist.

### CONTEXT
We need to integrate and configure the following MCP server for our environment:
**TARGET MCP SERVER & PURPOSE:** `[INSERT MCP SERVER NAME & WHAT WE WANT TO USE IT FOR]`

Follow **`ARCHITECTURE.md`** and **`agents_coding_rules.md`** strictly throughout this setup.

### WORKFLOW (Execute strictly in this order):

1. **Capability & Security Reconnaissance (Do This First):**
   - Inspect existing MCP configuration files in the workspace (e.g., `.cursor/mcp.json`, `.mcp.json`, or app-level MCP client configs) so you do not overwrite existing servers.
   - Identify:
     - **Exposed Primitives:** What specific Tools, Resources, and Prompts this MCP server exposes.
     - **Project-Specific Scope:** Which exact capabilities our project actually needs vs. which tools should remain disabled or restricted (e.g., read-only vs. write/destructive tools).
     - **Transport Mechanism:** Whether this server connects via `stdio` (`npx` / `uvx` / Docker) or remote `SSE` / `Streamable HTTP`.
     - **Credentials & Permissions:** Exact environment variables, API keys, or local paths required.

2. **Least-Privilege Configuration:**
   - Configure the MCP server entry using the cleanest, official package/command.
   - **Strict Secret Isolation:** Reference credentials via environment variables or secure local config files ignored by Git. **Immediately verify that any file containing real keys or tokens is added to `.gitignore` and documented safely with placeholder values in `.env.example`.**
   - Apply least-privilege flags where supported (e.g., `--read-only`, scoping filesystem access strictly to the project root, or scoping database MCP access to the development branch/schema).

3. **Connection & Tool Verification:**
   - Test the MCP server initialization to verify there are no missing runtime dependencies (Node/Python/`uv`/`npx` path issues).
   - Execute a safe, **read-only** diagnostic tool call (such as listing schemas, fetching docs, or checking status) to prove the connection is live and responding properly.

4. **Agent Usage Rules Update (If Applicable):**
   - Briefly document in `agents_coding_rules.md` (or our project docs) **when and how** coding agents should use this MCP server, and explicitly list any destructive MCP tools that require human confirmation before execution.

### STRICT RULES
- **Zero Committed Secrets:** Never hardcode raw API tokens, database connection strings with passwords, or personal access tokens into tracked Git files.
- **Least Privilege Always:** Never grant an MCP server write access to production databases or root filesystem directories outside this repository.
- **No Unrelated Architecture Changes:** Configuring a developer MCP tool must not alter application runtime dependencies unless the app itself is an MCP host/client specified in `ARCHITECTURE.md`.

### OUTPUT & HANDOFF
Once the MCP configuration is complete, provide a concise report covering:
1. **Connection Status:** Live & verified vs. waiting on user credentials.
2. **Configuration Added:** Which config file was updated and how `.gitignore` / `.env.example` were protected.
3. **Available Tools:** List of enabled MCP tools relevant to our project (and any dangerous tools intentionally restricted).
4. **Verification Result:** Output of the safe read-only test call.
5. **Manual Action Required:** Any API keys I need to paste or IDE window reloads required on my end.