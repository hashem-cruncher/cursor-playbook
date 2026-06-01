# Prompting Methodology & AI Communication 

##  Purpose
To establish a standardized, highly efficient methodology for communicating with AI models within Cursor. This approach minimizes ambiguity, enforces architectural constraints, and guarantees enterprise-grade, minimalistic code generation on the first attempt.

##  1. The Anatomy of a Perfect Prompt (CTCF Framework)
Every complex prompt in Composer or Chat must follow the **CTCF** structure:
1. **[C] Context:** What are we working on? (Use `@` references).
2. **[T] Task:** What exactly needs to be done? (Action-oriented verbs).
3. **[C] Constraints:** What are the strict rules? (Design patterns, things to avoid).
4. **[F] Format:** How should the AI deliver the output? (e.g., Code blocks only, step-by-step plan).

##  2. Advanced Prompting Techniques

### Chain of Thought (CoT) Prompting
For complex architectural changes or algorithmic logic, force the AI to explain its reasoning *before* writing code. 
- **Trigger Phrase:** *"Think step-by-step before implementing."* or *"Write a brief technical implementation plan first. Wait for my approval before generating the code."*

### Negative Prompting (The "Do Not" Rule)
AI models often try to be "too helpful" by adding unnecessary libraries or styles. Explicitly state what you *don't* want.
- **Example:** *"Do not use Redux for this. Do not write inline CSS. Do not modify the existing database schema."*

### Role-Based Priming
Prime the AI to adopt a specific persona to elevate the quality of the response.
- **Trigger Phrase:** *"Act as a Staff Security Engineer..."* or *"As an expert AI Engineer and System Architect..."*

## ❌ vs ✅ Real-World Examples

###  Bad Prompt (Vague, High Token Waste, Prone to Hallucination)
> *"Create a login page with email and password and connect it to the backend. Make it look good."*
> **Why it fails:** The AI will guess the UI framework, guess the backend API structure, likely write custom CSS instead of your utility classes, and output 3 different files you didn't ask for.

###  Good Prompt (Structured, Precise, Enterprise-Grade)
> *"I need to build the `LoginPage` component.*
> 
> **Context:** Reference `@project-context.md` for our stack. Review `@auth_service.py` (FastAPI backend) for the expected payload.
> **Task:** Create a React component using Next.js App Router that handles email/password submission.
> **Constraints:** > - Use a minimalistic, enterprise-grade design.
> - Use standard Tailwind utility classes.
> - Handle loading states and display error messages returned from the API.
> - Do not add any new npm packages.
> **Format:** Output the component code only. No explanations needed."

##  3. Daily Quick-Prompt Templates

**For Debugging:**
> "Analyze the error trace provided. Review `@target_file.ts`. Identify the root cause, explain why it happened in one sentence, and provide the minimal code snippet to fix it."

**For Refactoring:**
> "Refactor this highlighted function to improve readability and reduce Big O time complexity. Apply clean code principles. Ensure typing is strict."

**For Code Review:**
> "Act as a strict Senior Reviewer. Review this current file for security vulnerabilities, memory leaks, and adherence to clean architecture. Provide a bulleted list of issues."

##  4. Handling Hallucinations or Mistakes
If the AI outputs incorrect code, **do not** write *"You are wrong."* Instead, gently but firmly realign it using facts:
- *"This implementation uses X, but our architecture requires Y. Rewrite using Y."*
- *"You modified `utils.py` which was out of scope. Revert those changes and only provide the fix for `main.py`."*