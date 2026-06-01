# Code Review & Quality Assurance Workflow

## Goal
To utilize Cursor AI as an objective, strict Senior Technical Reviewer. This workflow identifies security vulnerabilities, performance bottlenecks, architectural deviations, and logic flaws before code is merged into the main branch or deployed.

## When To Use
- Before opening a Pull Request (PR).
- After completing a complex feature implementation.
- When inheriting legacy code that lacks documentation.

## Step-by-Step Workflow

### Phase 1: Context Preparation (Human Task)
1. **Finalize Changes:** Ensure the feature is complete and runs locally without obvious runtime errors.
2. **Review Diff:** Use source control to view the diff. Identify which specific files contain the core logic changes.

### Phase 2: AI Code Review (Cursor Chat)
Initiate a comprehensive review session.
1. **Open New Chat:** Clear previous contexts to ensure maximum token allocation for the review.
2. **The Review Prompt:**
   > "Act as a strict Senior Software Engineer. I am preparing a Pull Request.
   > **Task:** Review the changes in @[Target_File_1] and @[Target_File_2].
   > **Focus Areas:**
   > - Security vulnerabilities (e.g., input validation, exposure of sensitive data).
   > - Performance issues (e.g., memory leaks, unoptimized loops, blocking operations).
   > - Adherence to our architecture defined in @project-context.md and @ai-behavior-rules.md.
   > - Edge cases not handled properly.
   > **Constraint:** Do not rewrite the files. Provide a bulleted list of issues classified by severity (Critical, Warning, Suggestion). If the code is solid, explicitly state 'No major issues found'."

### Phase 3: Triaging and Resolution
Address the AI's findings methodically.
1. **Evaluate Findings:** The AI might flag false positives. Use engineering judgment to decide which issues require immediate action.
2. **Request Fixes:** For valid issues, ask the AI for the specific implementation fix.
   > "Your finding regarding the unoptimized database query in `get_metrics` is correct. Provide the inline code replacement to implement eager loading and resolve this bottleneck."
3. **Apply Fixes:** Use Inline Edit to apply the targeted solutions.

### Phase 4: Self-Check Techniques
Before finalizing the review, use prompt-based self-checks to ensure nothing was missed.
- **The Edge Case Check:** "Identify 3 edge-case inputs that could cause this specific function to fail or return unexpected results."
- **The Docker/Environment Check:** "Does this code rely on any local machine configurations that would fail when deployed in an isolated containerized environment?"

## Code Review Checklist
- [ ] Business logic is isolated from routing/UI components.
- [ ] Strict typing is maintained; no implicit fallback types.
- [ ] Environment variables are utilized securely.
- [ ] Error handling is comprehensive and does not leak system paths.
- [ ] Code is formatted according to project standards.

## Token Optimization
- Do not ask the AI to review the entire repository. Only feed it the files that have been modified in the current branch.
- If reviewing a massive PR, break the review into domains (e.g., "Review the frontend components first. I will ask for the backend service review next.").