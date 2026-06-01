# Debugging Prompts Library

## Purpose
A collection of highly targeted, reusable prompts designed to diagnose and fix bugs efficiently. These prompts force the AI to perform Root Cause Analysis (RCA) before writing any code, ensuring that fixes address the underlying issue rather than applying a superficial patch.

## When To Use
- Application crashes or unhandled exceptions.
- Logical errors where the output does not match expected business rules.
- UI rendering issues or state synchronization bugs.
- Performance bottlenecks or memory leaks.

---

## 1. The Stack Trace / Crash Analyzer Prompt
*Use this when the application throws a fatal error or backend exception.*

**Prompt Template:**
```text
Task: Diagnose and resolve the following stack trace.

Error Log:
[PASTE EXACT, TRUNCATED ERROR LOG HERE]

Context:
- The error seems to originate in or around @[File_Name].
- Review @ai-behavior-rules.md for our strict error handling policies.

Requirements:
1. Root Cause Analysis: Explain in 2 concise sentences exactly why this error occurred. Do not guess; if context is missing, ask me.
2. Minimal Fix: Provide the exact code block needed to fix the issue.
3. Regression Check: Briefly mention if this fix could impact any other dependent functions.

Constraint: Output only the corrected code blocks. Do not rewrite the entire file. Maintain all existing imports and function signatures unless they are the cause of the bug.

```

---

## 2. The Logical Bug / Misbehavior Prompt

*Use this when the code compiles and runs, but the business logic or calculation is incorrect.*

**Prompt Template:**

```text
Task: Fix a logical discrepancy in @[File_Name].

Context:
- Expected Behavior: [e.g., The invoice total should include a 16% tax rate only for domestic clients].
- Actual Behavior: [e.g., The tax is being applied to all clients, or the total is returning null].

Requirements:
1. Trace the data flow through the @[Function_Name] function.
2. Identify where the logic deviates from the Expected Behavior.
3. Provide the corrected code implementation.
4. Suggest one edge case we should add a unit test for to prevent this in the future.

Constraint: Preserve the overall architecture. Only modify the specific conditional blocks or calculations causing the failure.

```

---

## 3. The Frontend State / UI Glitch Prompt

*Use this for React/Next.js rendering issues, infinite loops, or CSS misalignments.*

**Prompt Template:**

```text
Task: Resolve a UI/State bug in @[Component_Name.tsx].

Context:
- Issue description: [e.g., The component re-renders infinitely when the modal is closed, OR The flexbox layout breaks on mobile screens].
- Related state/store: Review @[State_Store_File] if necessary.

Requirements:
1. Analyze the component lifecycle and dependency arrays (if using hooks like useEffect).
2. Identify the trigger causing the unintended behavior.
3. Provide the minimal code fix. 

Constraint: Do not introduce new third-party UI libraries. Use existing Tailwind classes or our established design system. Ensure strict TypeScript types are maintained.

```

---

## 4. The Performance Bottleneck Prompt

*Use this when an endpoint is slow or a database query takes too long.*

**Prompt Template:**

```text
Task: Optimize a performance bottleneck in @[File_Name].

Context:
- The function @[Function_Name] is currently taking [X] seconds to execute / returning a large payload.
- We suspect the issue is related to [e.g., N+1 query problem, synchronous blocking calls, unoptimized loop].

Requirements:
1. Analyze the time complexity (Big O) of the current implementation.
2. Provide an optimized version of the code (e.g., implementing eager loading, pagination, or asynchronous processing).
3. Explain briefly why the new version is faster.

Constraint: The optimized code must return the exact same payload structure as the original to avoid breaking client contracts.

```

---

## Best Practices for Debugging Prompts

* **Limit the Scope:** Only mention the files directly involved in the error. Including unrelated files will confuse the AI and waste tokens.
* **Provide Exact Variables:** If a variable is returning `undefined`, state the exact name of the variable in the prompt.
* **Reject "Band-Aids":** If the AI suggests silencing an error (e.g., adding `// @ts-ignore` or wrapping in a broad `try/except pass`), explicitly reject it and ask for a proper architectural fix.
