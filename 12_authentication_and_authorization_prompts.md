# 12. Authentication & Authorization Prompt (`authentication_and_authorization_prompts.md`)

> **How to use:** Paste this prompt into your coding agent during Phase 2 of your `IMPLEMENTATION_PLAN.md` when you are ready to implement user identity, sessions, protected routes, and role-based access control (RBAC).

---

## Prompt

Act as a Principal Security-Focused Full-Stack Engineer specializing in production authentication, session management, and access-control systems.

### CONTEXT
Your strict sources of truth for this task are **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**.

### TASK
Implement the complete, end-to-end authentication and authorization system for this application exactly as specified in our architectural blueprints.

Execute the following workflow step-by-step:

1. **Reconnaissance & Existing Setup Check:**
   - Inspect the codebase first to see if any auth configuration, middleware, or user schema already exists.
   - Verify the exact auth provider/strategy locked in `ARCHITECTURE.md` (e.g., NextAuth/Auth.js, Supabase Auth, Clerk, Better-Auth, or custom JWT/Session with FastAPI/Express). **Do NOT switch providers or invent a custom auth scheme if an approved strategy is already defined.**

2. **Identity & Credential Flows:**
   - Implement the required onboarding and entry flows specified in `PRD.md` (e.g., Sign Up, Log In, Log Out, OAuth providers, Magic Links, or Password Reset).
   - **Password & Token Security (If applicable):** Use industry-standard cryptographic hashing (`Argon2id` or `bcrypt` with appropriate work factor). Never store or log plaintext passwords, tokens, or secrets.
   - **Input Validation & Rate Limiting:** Enforce strict schema validation on all auth payloads and apply rate-limiting rules from `API.md` to prevent brute-force and credential-stuffing attacks.

3. **Session & Token Lifecycle Management:**
   - Implement secure session storage (`HttpOnly`, `Secure`, `SameSite=Lax/Strict` cookies or short-lived access tokens + rotation).
   - Handle session expiration, silent token refresh, and complete server-side session invalidation on logout.

4. **Server-Side Route Protection & Middleware:**
   - Configure edge/routing middleware to guard protected frontend routes defined in `PRD.md` (redirecting unauthenticated users cleanly to the login screen with a safe `callbackUrl` / `redirectTo` parameter).
   - Build reusable server-side auth guards/dependencies for API endpoints and Server Actions that verify session validity before executing any business logic.

5. **Role-Based Access Control (RBAC) & Resource Ownership (IDOR Prevention):**
   - Implement the exact user roles and permission matrix from `PRD.md` and `DATABASE.md`.
   - **Mandatory Ownership Verification:** Ensure every read, update, or delete operation explicitly checks that the authenticated user owns the target resource (`user_id` / `organization_id` match) or holds an authorized `admin` role. Never trust a `userId` or `role` passed from the client body/query.

6. **Frontend Auth UI & Feedback States:**
   - Build the Auth screens/forms strictly following `DESIGN.md` tokens, including inline field validation, loading states on submit buttons, and clear error banners (using safe, generic error messages where necessary to prevent user enumeration).
   - Implement clean `401 Unauthorized` (session expired/unauthenticated) and `403 Forbidden` (insufficient permissions) fallback views.

7. **Automated Verification & Security Tests:**
   - Write integration tests covering:
     - Successful signup/login/logout lifecycle.
     - Unauthenticated requests to protected APIs returning `401`.
     - Authenticated user attempting to access or mutate another user's resource returning `403` or `404`.

### STRICT RULES
- **Zero Client-Side Trust:** Client-side UI hiding (e.g., hiding an "Admin" button in React) is UX, **not** security. Every single permission check must be enforced on the server/database layer.
- **No Secret Leakage:** Never expose auth secrets, signing keys, service-role tokens, or password hashes in client bundles, API responses, or console logs.
- **Scope Discipline:** Do not modify unrelated business logic or install unapproved auth packages.

### OUTPUT & HANDOFF
Once authentication and authorization are implemented and verified:
1. Make an atomic Git commit (`feat: implement authentication and RBAC system`).
2. Provide a concise security report listing:
   - The auth flows implemented.
   - Which routes and API endpoints are now protected.
   - How RBAC and resource ownership are enforced.
   - The results of your auth tests and any edge cases requiring my review.