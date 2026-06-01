# Debugging & Issue Resolution Workflow

## Goal
To systematically identify, analyze, and resolve codebase errors using Cursor AI without degrading existing code quality, introducing unintended side effects, or wasting tokens on massive file rewrites.

## When To Use
- Resolving runtime errors or stack traces.
- Fixing logical bugs or unexpected behavior in the application.
- Addressing build failures or deployment issues.
- Correcting UI/UX rendering glitches.

## Step-by-Step Workflow

### Phase 1: Isolation & Preparation (Human Task)
Do not immediately paste the error into Cursor Chat. 
1. **Identify the Scope:** Determine if the error is Frontend, Backend, Database, or Infrastructure.
2. **Truncate Logs:** Copy only the relevant portion of the stack trace. Remove boilerplate system logs or repetitive loop errors.
3. **Locate the Suspect:** If the stack trace points to a specific file or line number, open that file in your editor.

### Phase 2: Root Cause Analysis (Cursor Chat)
Force the AI to think before it writes code.
1. **Open Chat:** Start a completely new chat to ensure a clean context window.
2. **The Diagnostic Prompt:**
   > "Review the following error log: [Paste Truncated Log]. 
   > The error seems to originate in `@target_file.ext`. 
   > **Task:** Analyze the code and identify the exact root cause of this issue. 
   > **Constraint:** Do not write the code to fix it yet. Only explain *why* this is failing in 2-3 sentences."
3. **Validate:** Read the AI's explanation. If the logic makes sense, proceed to the fix. If it sounds like a hallucination, redirect the AI by pointing out architectural facts.

### Phase 3: Minimal Intervention Fix
Once the root cause is confirmed, request the most precise fix possible.
1. **The Fix Prompt:**
   > "Your analysis is correct. Now, provide the minimal code changes required to fix this issue in `@target_file.ext`. 
   > **Constraints:** 
   > - Do not rewrite the entire file. 
   > - Do not change the overall architecture or function signatures unless absolutely necessary.
   > - Output only the specific blocks of code that need to be replaced."

### Phase 4: Implementation & Verification (Inline or Composer)
1. Use **Inline Edit (CMD/CTRL + K)** on the specific function that needs changing, using a short prompt like: "Apply the fix we discussed in the chat to resolve the null reference."
2. Run your local tests or trigger the action that caused the bug to verify the fix.
3. Check for regressions in related components.

## AI Interaction Strategy
- **The "Rubber Duck" Approach:** Treat the AI as a senior colleague. Explain what the system *should* be doing versus what it *is* doing.
- **Defensive Prompting:** Remind the AI to maintain existing imports and variable names during the fix.

## Context Management & Token Optimization
- **Never paste entire log files.** Truncate them to the specific exception and the top 3-4 lines of the stack trace.
- **Close irrelevant tabs.** If you have 15 files open, Cursor might index them as context. Close files unrelated to the bug before asking the AI to analyze the workspace.
- **Start fresh.** If a debugging session takes more than 5 turns and the code is getting messy, revert your git changes, start a new chat, and try a different approach based on what you learned.

## Best Practices
- **Write Tests First:** If applicable, ask the AI to write a failing unit test that reproduces the bug before asking it to fix the bug.
- **Read the Docs:** If the error is related to a specific library version, use `@Web` to force the AI to read the latest documentation before guessing the fix.

## Common Mistakes
- **The "Fix It" Command:** Pasting an error code and just typing "fix this". The AI will guess the context and often rewrite unrelated code.
- **Blind Acceptance:** Accepting an AI fix that solves the error but introduces a severe security vulnerability or performance bottleneck (e.g., removing authentication to fix a 403 error).
- **Chasing Hallucinations:** Following the AI down a rabbit hole of installing new npm packages or python libraries just to fix a simple syntax or logic error.