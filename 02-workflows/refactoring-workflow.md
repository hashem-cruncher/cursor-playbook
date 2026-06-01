# Refactoring Workflow

## Goal
To systematically restructure existing code without changing its external behavior. This workflow leverages AI to reduce technical debt, improve readability, and enforce clean architecture while strictly mitigating the risk of introducing regressions or wasting tokens.

## When To Use
- Breaking down monolithic functions or components into smaller, modular pieces.
- Upgrading legacy logic to modern standards.
- Decoupling business logic from routing layers or UI components.
- Optimizing database queries or state management performance.

## Step-by-Step Workflow

### Phase 1: Isolation and Baseline (Human Task)
1. **Commit Current State:** Ensure the working directory is clean. Commit any unsaved changes.
2. **Identify Scope:** Select the specific file or block of code to be refactored. Do not attempt to refactor multiple domains simultaneously.
3. **Verify Tests:** If applicable, ensure existing unit or integration tests for this module are passing before making any changes.

### Phase 2: Analysis and Strategy (Cursor Chat)
Force the AI to evaluate the current state and propose a plan before touching the codebase.
1. **Open New Chat:** Use a high-reasoning model.
2. **The Strategy Prompt:**
   > "Review the code in @[Target_File]. 
   > **Task:** We need to refactor this to improve [mention specific goal: e.g., readability, separation of concerns, time complexity].
   > **Constraint:** Do not write the new code yet. Identify the main pain points in the current implementation and propose a step-by-step refactoring plan. Ensure the plan aligns with our @ai-behavior-rules.md."
3. **Review Plan:** Approve or adjust the phases proposed by the AI.

### Phase 3: Incremental Execution (Composer or Inline)
Execute the refactor in isolated steps based on the approved plan.
1. **Execute Step 1:** Use Composer or Inline Edit (CMD/CTRL + K).
   > "Execute Step 1 of our refactoring plan for @[Target_File]. Output only the modified code blocks. Maintain all existing function signatures and data contracts."
2. **Verify:** Check for syntax errors and ensure the code compiles.
3. **Iterate:** Proceed to Step 2, and so on. Do not rush the AI to do all steps at once.

### Phase 4: Final Verification
1. Run application tests.
2. Manually trigger the refactored functionality in the development environment.
3. Commit the refactored code with a descriptive message outlining the structural changes.

## AI Interaction Strategy
- **Enforce Parity:** Explicitly remind the AI: "The external output of this function must remain 100% identical to the original."
- **Variable Naming:** Instruct the AI to use descriptive, domain-specific naming conventions during the rewrite to make the code self-documenting.

## Common Mistakes
- **The "Rewrite This" Trap:** Highlighting a 500-line file and pressing CMD+K with "refactor this". The AI will hallucinate dependencies and break the application.
- **Losing Business Logic:** Allowing the AI to delete edge-case handling simply because it makes the code look "cleaner".