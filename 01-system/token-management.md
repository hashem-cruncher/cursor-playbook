# Token Management & Context Optimization 

##  Purpose
To define strict strategies for managing the LLM context window within Cursor AI. Efficient token management prevents "AI amnesia", reduces API costs/usage limits on premium models, and significantly improves the precision of generated code.

##  1. The Core Philosophy: "Surgical Precision"
- **Context is Expensive:** Treat every token as a limited resource. Do not dump the entire project into the context window.
- **High Signal, Low Noise:** Only provide the AI with the exact files, functions, or documentation necessary to solve the immediate problem.
- **Fresh Context = Better Output:** Long chat threads degrade AI performance. Once a specific task or bug is resolved, start a **New Chat** for the next task.

##  2. Cursor-Specific Token Strategies

### Use `.cursorignore` Effectively
Prevent Cursor's codebase indexing from wasting tokens on irrelevant files. Ensure your `.cursorignore` (or `.gitignore`) excludes:
- `node_modules/`, `venv/`, `env/`
- Build outputs (`dist/`, `.next/`, `build/`)
- Log files (`*.log`)
- Large datasets, CSVs, or model weights (`*.csv`, `*.pt`, `*.bin`, `.parquet`)
- Compiled binaries and media assets

### Master the `@` Symbol
Never type out "Look at the user service file". Always use precise `@` mentions:
- `@Files`: Mention specific files (e.g., `@main.py`, `@auth.ts`).
- `@Folders`: Mention a specific domain if asking architectural questions (e.g., `@src/components/ui`).
- `@Codebase`: **USE SPARINGLY.** Only use this for global searches or project-wide refactoring planning. It consumes massive amounts of tokens.
- `@Web`: Use this to fetch the latest documentation for a specific library version, avoiding outdated training data hallucinations.

###  3. Chat vs. Composer vs. Inline (CMD/CTRL + K)
Choose the right tool to minimize token overhead:
1. **Inline Edit (CMD/CTRL + K):** Use for precise, single-file edits (e.g., "Extract this inline style to a Tailwind class" or "Refactor this function to be async"). *Lowest Token Cost.*
2. **Chat (CMD/CTRL + L):** Use for planning, debugging, and asking questions across 2-3 files.
3. **Composer (CMD/CTRL + I):** Use for generating scaffolding or multi-file features. Always provide a clear prompt and reference the `@project-context.md` to ensure it generates code correctly the first time. *Highest Token Cost.*

##  4. Handling Large Files & Refactoring
If a file exceeds 500 lines, the AI will struggle to rewrite it efficiently.
- **Do NOT ask:** "Refactor this entire file."
- **DO ask:** "Review the `calculate_metrics` function from lines 120-185 and optimize the database query. Only output the updated function."
- **Chunking:** Break down large tasks. Instruct the AI: "We are going to refactor this module in 3 steps. I will guide you through each step. Acknowledge and wait for step 1."

##  5. Quick Token Checklist Before Hitting 'Enter'
- [ ] Is this a new task? If yes, clear the chat / start a new one.
- [ ] Did I explicitly `@` mention only the relevant files?
- [ ] Am I asking the AI to output only the modified code blocks instead of the whole file?
- [ ] Is my prompt specific enough to avoid the AI "guessing" and generating useless code?

##  6. Common Token Wasters
- **"Fix this error":** Pasting a stack trace without pointing to the relevant file. The AI will scan the whole project.
- **Endless Follow-ups:** Continuing a chat after 10+ turns. The context is polluted with old code. Start fresh and summarize the current state.
- **Over-Prompting:** Writing a 500-word prompt for a 10-line code change. Keep instructions dense and technical.