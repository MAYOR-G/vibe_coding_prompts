# 16. Debugging & Root-Cause Analysis Prompt (`debugging_root_cause_prompts.md`)

> **How to use:** Paste this prompt into your coding agent whenever you hit a bug, runtime crash, build failure, broken UI state, or failing test. Replace `[INSERT ERROR / BUG / STACK TRACE / SCREENSHOT DESCRIPTION]` with the actual issue before sending.

---

## Prompt

Act as a Principal Debugging & Systems Reliability Engineer specializing in systematic root-cause analysis inside production codebases.

### CONTEXT
Follow our approved **`ARCHITECTURE.md`**, **`DATABASE.md`**, **`API.md`**, and **`agents_coding_rules.md`**, and respect all existing patterns in the codebase.

### PROBLEM STATEMENT
Investigate and resolve the following issue:
**BUG / ERROR / SYMPTOM:**
`[INSERT ERROR MESSAGE, STACK TRACE, LOG OUTPUT, OR SCREENSHOT DESCRIPTION HERE]`

### SYSTEMATIC DEBUGGING WORKFLOW (Execute strictly in this order):

1. **Trace & Reproduce (Do Not Guess):**
   - Locate the exact file(s), function(s), and line numbers referenced in the error or involved in the broken user flow.
   - Trace the execution path end-to-end (UI component -> client state/fetch -> API route/Server Action -> validation schema -> database query -> response serialization).
   - Reproduce the failure deterministically (via a failing test, terminal reproduction command, or step-by-step call trace).

2. **Evidence-Based Root-Cause Diagnosis:**
   - Inspect data types, `null`/`undefined` boundaries, async/await race conditions, environment variable availability, ORM query mismatches, and client/server hydration or serialization boundaries.
   - Formulate and verify the **actual root cause**—distinguishing the underlying defect from surface-level symptoms.
   - *Rule:* If the evidence is insufficient to confirm the root cause with 100% certainty, **STOP** before editing application logic. Tell me what diagnostic log or check is needed first.

3. **Explain Before Patching:**
   - Briefly state in plain technical terms:
     1. **What** is failing.
     2. **Why** it is happening (the exact mechanism).
     3. **Where** the boundary broke against our specs.

4. **Implement the Minimal Surgical Fix:**
   - Apply the smallest, cleanest code change that resolves the root cause at its source.
   - Check if the exact same anti-pattern exists in sibling functions or routes, and flag or fix them if directly related.

5. **Regression & Verification Testing:**
   - Add or update a targeted test that fails without your fix and passes with your fix.
   - Run the type checker, linter, and existing test suite to guarantee zero regressions were introduced.

### STRICT RULES
- **Zero Blind Trial-and-Error:** Do not randomly tweak code, swap packages, or stack workarounds hoping the error disappears.
- **Never Mask or Silence Errors:** You are strictly forbidden from wrapping broken logic in empty `try/catch {}` blocks, adding `@ts-ignore` / `any`, weakening Zod/Pydantic validation, or deleting failing tests to make the error go away.
- **No Unrelated Refactoring:** Touch only the lines required to fix the bug. Do not reformat unrelated files or alter existing features.

### OUTPUT & HANDOFF
Once the bug is resolved and verified, provide a structured **Root-Cause Report**:
1. **Root Cause Analysis:** Exact explanation of why the bug occurred (file, line, and faulty assumption).
2. **Surgical Fix Applied:** Summary of the code changes made and files modified.
3. **Verification & Regression Check:** Commands/tests run to prove the bug is fixed and nothing else broke.
4. **Remaining Risk / Prevention:** Any edge cases or preventative checks to watch out for.