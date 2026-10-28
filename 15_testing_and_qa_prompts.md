# 15. Testing & QA Verification Prompt (`testing_and_qa_prompts.md`)

> **How to use:** Paste this prompt into your coding agent after implementing a major feature phase or before preparing for launch. You can either run it across the whole MVP or scope it to a specific module by updating `[ALL IMPLEMENTED FEATURES / SPECIFIC MODULE]`.

---

## Prompt

Act as a Principal QA Automation Engineer and Production Reliability Specialist.

### CONTEXT
Read **`PRD.md`**, **`DESIGN.md`**, **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**, then inspect the current codebase implementation.

**TARGET TESTING SCOPE:** `[ALL IMPLEMENTED FEATURES / OR SPECIFY MODULE]`

### WORKFLOW (Execute strictly in this order):

1. **Test Environment & Coverage Audit (Do This First):**
   - Inspect the existing test setup, test runner configuration (e.g., Vitest, Jest, Pytest, Playwright), and existing test files.
   - Compare the implemented code against the **Acceptance Criteria** in `PRD.md` and the endpoint contracts in `API.md` to identify untested critical paths.

2. **Unit & Business Logic Tests (Fast & Isolated):**
   - Write deterministic unit tests for core domain logic, data transformers, utility functions, and validation schemas (Zod/Pydantic).
   - Explicitly test boundary conditions: `null`/`undefined` inputs, empty strings, malformed payloads, negative numbers, extreme character lengths, and special characters.

3. **API, Database & Security Integration Tests:**
   - Test every critical endpoint or Server Action against `API.md` contracts:
     - **Happy Path:** Valid payload -> expected status code (`200`/`201`), correct response envelope, and verified database state change.
     - **Validation Failures:** Invalid/missing required fields -> returns `400`/`422` with structured field errors and **zero** partial database writes.
     - **Auth & Ownership (IDOR) Checks:** Unauthenticated request -> `401`; Authenticated user attempting to read/mutate another user's record -> `403`/`404`.
     - **External Service Failure Handling:** Mock third-party APIs returning `429` (rate limit), `500` (outage), or network timeouts to verify graceful fallback behavior.

4. **End-to-End (E2E) & UI State Verification:**
   - Verify the P0 core user journeys defined in `PRD.md` from start to finish:
     - **UI Feedback States:** Confirm that **Loading** skeletons/disabled submit buttons, **Empty** state CTAs, and **Error** banners/inline messages render properly.
     - **Double-Submit Protection:** Verify that rapid clicks on submit/mutation buttons do not create duplicate records.
     - **Responsive & Accessibility Checks:** Verify mobile/desktop layout integrity and keyboard/form accessibility basics from `DESIGN.md`.

5. **Execute & Triage:**
   - Run the complete test suite, type checker, and linter in the terminal.
   - If a test fails, determine whether the bug is in the **test setup** or in the **application code**.

### STRICT RULES
- **Never Cheat the Tests:** You are strictly forbidden from weakening assertions, deleting failing tests, adding `@ts-ignore`, or changing production business logic just to force a green build.
- **No Live External Calls:** Always mock paid/rate-limited third-party APIs (LLMs, payment gateways, email providers) and isolate test database state so running tests never corrupts development or production data.
- **High-Signal Over Vanity Metrics:** Prioritize business-critical user flows, data integrity, and security boundaries over superficial snapshot tests.

### OUTPUT & HANDOFF
Once the test suite has been written and executed, provide a structured **QA Report**:
1. **Test Execution Summary:** Total tests run, passed, and failed (plus build/typecheck status).
2. **New Test Files Created:** List of unit, integration, and E2E test files added.
3. **Bugs & Vulnerabilities Discovered:** For any failure or bug uncovered in the application code, list:
   - **Severity:** (`Critical`, `High`, `Medium`, `Low`)
   - **Location:** Exact file path and function/line.
   - **Root Cause:** Why it fails against `PRD.md` / `API.md`.
   - **Recommended Fix:** Exact fix required (ask for my approval before modifying production code if the fix alters core architecture or schema).