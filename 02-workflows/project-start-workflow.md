# Project Start & Scaffolding Workflow 

##  Goal
To establish a rigid, systematic approach for initializing new projects or major features using Cursor AI. This workflow ensures the AI fully understands the architecture, prevents initial hallucinations, and generates scalable boilerplate code without token waste.

##  When To Use
- Starting a brand new repository.
- Initiating a massive new feature (e.g., adding an entire Admin Dashboard or a new Microservice).

##  Step-by-Step Workflow

### Phase 1: Context & Environment Setup (Human Task)
*Do not start chatting with the AI until these steps are completed.*
1. **Initialize Repo:** Create the folder structure and initialize git.
2. **Setup `.cursorignore`:** Immediately add `.cursorignore` to exclude build files, node_modules, and virtual environments to save indexing tokens.
3. **Inject Context:** Copy `@project-context-template.md` to the root, rename it to `project-context.md`, and fill in the specifics of this new project/feature.
4. **Link Rules:** Ensure `ai-behavior-rules.md` is accessible in your workspace.

### Phase 2: Architectural Planning (Cursor Chat - Smart Model)
*Use a high-reasoning model (e.g., Claude 3.5 Sonnet or GPT-4o).*
1. **Open a New Chat** (CMD/CTRL + L).
2. **The Initial Prompt:**
   > "I am starting a new project/feature. Read `@project-context.md` and our `@ai-behavior-rules.md`. 
   > **Task:** Act as a Software Architect. Draft a file-tree structure and a brief technical implementation plan for the initial setup. 
   > **Constraint:** Do not write any functional code yet. Only output the proposed architecture and package requirements. Wait for my approval."
3. **Review & Refine:** Adjust the AI's proposal. Do not proceed until the architecture is 100% correct.

### Phase 3: Scaffolding (Cursor Composer)
*Once the plan is approved, switch to execution mode.*
1. **Open Composer** (CMD/CTRL + I - Full Screen is recommended for scaffolding).
2. **The Scaffolding Prompt:**
   > "Based on the architectural plan we just agreed upon in the chat, generate the initial scaffolding for the project.
   > **Requirements:**
   > - Create the core folder structure.
   > - Setup the configuration files (e.g., `package.json`, `docker-compose.yml`, `requirements.txt`).
   > - Add the base routing or main entry point (e.g., `main.py` or `app/layout.tsx`).
   > - Follow the enterprise-grade, minimalistic design standards outlined in the context."
3. **Accept/Reject:** Review the generated files in Composer before hitting 'Accept All'.

### Phase 4: Baseline Verification
1. Run the project locally (`npm run dev`, `docker-compose up`, etc.).
2. Verify there are no missing dependencies or syntax errors in the boilerplate.
3. Commit the initial scaffolding: `git commit -m "chore: initial project scaffolding via AI"`.

##  AI Interaction Strategy
- **Separate Thinking from Typing:** Force the AI to act as an architect first (Chat), then as a coder (Composer).
- **Control the Reins:** Never let the AI generate 20 files at once without a pre-approved plan. It *will* hallucinate dependencies or mix architectural patterns.

##  Before-Start Checklist
- [ ] `project-context.md` is filled out and accurate.
- [ ] `.cursorignore` is configured.
- [ ] Tech stack versions are explicitly defined to avoid legacy code generation.
- [ ] The human developer has a clear mental model of the end goal before prompting the AI.

##  Common Mistakes to Avoid
- **The "Do Everything" Prompt:** Asking the AI to "Build a full e-commerce app" in one prompt.
- **Skipping the Context File:** Forcing the AI to guess the preferred tech stack, resulting in Next.js Pages router instead of App router, or Flask instead of FastAPI.
- **Ignoring Dependency Conflicts:** Blindly accepting `package.json` generations without checking version compatibility.