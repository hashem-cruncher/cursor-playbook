# Cursor AI Engineering Playbook

This repository serves as the central "Operating System" for AI-assisted full-stack development. It contains strict rules, structured workflows, and production-grade prompts designed to maximize the efficiency of Cursor AI, reduce token consumption, and enforce enterprise-grade architectural standards.

---

## 📌 Daily Quick Access (High Priority)
Use these links daily to inject context or pull specific prompts without leaving your IDE.

| Resource | Purpose | Path |
| :--- | :--- | :--- |
| **Project Context** | Always reference this first in new chats. | [`@project-context.md`](./04-context/project-context-template.md) |
| **AI Behavior Rules** | The core constraints for code generation. | [`@ai-behavior-rules.md`](./05-rules/ai-behavior-rules.md) |
| **Token Management** | Strategies to avoid context bloat. | [`@token-management.md`](./01-system/token-management.md) |
| **Debugging Prompts** | Copy/paste templates for stack traces. | [`@debugging-prompts.md`](./03-prompts/debugging-prompts.md) |
| **Model Selection** | When to use Fast vs. Smart models. | [`@model-selection.md`](./01-system/model-selection.md) |

---

## 🚀 Project Initialization: New vs. Existing

Before writing any code, the AI must understand the environment. Follow the correct path based on your project state.

### Scenario A: Starting a Brand New Project
1. **Context & Rules:** Copy `@project-context-template.md` to the root as `project-context.md`. Copy the contents of `@ai-behavior-rules.md` into a local `.cursorrules` file.
2. **Protection:** Setup `.cursorignore` immediately.
3. **Planning Chat:** Use Chat (Smart Model) with the prompt: *"Read `@project-context.md`. Draft a file-tree structure and implementation plan. Do not write code yet."*
4. **Scaffolding:** Once approved, use Composer to generate the boilerplate. 
*(Full details in [`@project-start-workflow.md`](./02-workflows/project-start-workflow.md))*

### Scenario B: Onboarding a Legacy / Existing Project
Applying these strict rules to an existing codebase requires a phased approach to prevent conflicts with existing technical debt.
1. **Rules Injection:** Create a `.cursorrules` file in the root and paste the contents of `@ai-behavior-rules.md`.
2. **Reality-Based Context:** Copy `@project-context-template.md` to the root. **Crucial:** Clearly state the *Current Status* (e.g., "Legacy codebase with technical debt. Goal is phased refactoring").
3. **Token Quarantine:** Ensure `.cursorignore` aggressively blocks old build folders, virtual environments, and large media files.
4. **The Onboarding Chat:** Open a new Chat (Smart Model) and use this exact prompt:
   > *"I am introducing you to an existing codebase. Read `@project-context.md`. Review the core files in the main directory. **Task:** Summarize the architecture and identify 3 major technical debts or rule violations based on our `.cursorrules`. **Constraint:** Do not write any new code or attempt to fix them yet."*
5. **Incremental Application:** Do not ask the AI to "refactor the project". Use the [`@refactoring-workflow.md`](./02-workflows/refactoring-workflow.md) to update one file or module at a time safely.

---

## ⚙️ Daily Operating Workflow

### 1. Feature Development
- Use **Chat (Smart Model: Claude 3.5 Sonnet)** for planning and architectural decisions.
- Use **Composer (Smart Model)** for scaffolding multi-file features based on the agreed plan.
- Reference domain rules: Tell the AI to strictly follow `@frontend-rules.md` or `@backend-rules.md`.

### 2. Debugging
- Never paste a raw error without context.
- Use the **Debugging Workflow**: Truncate the stack trace and use the templates from [`debugging-prompts.md`](./03-prompts/debugging-prompts.md).
- Force the AI to explain the *Root Cause* before generating the fix to prevent architectural regression.

### 3. Code Review & Commit
- Before committing, run manual verification against the [`before-commit.md`](./06-checklists/before-commit.md) checklist.
- Highlight the diff and use a **Fast Model** (e.g., Haiku) to generate conventional commit messages.

---

## 💰 Maximum Quality, Minimum Cost (Token Optimization)

To preserve your premium fast-request quotas and ensure the AI remains highly accurate:

1. **Precision Mentions:** Use `@filename` instead of typing "Look at the auth service".
2. **Avoid `@Codebase`:** Only use codebase-wide search for global refactoring. It consumes massive token counts.
3. **The Downgrade Rule:** Use Smart Models (Claude 3.5 Sonnet) for logic, RCA, and architecture. Switch immediately to Fast Models (Claude 3.5 Haiku / GPT-4o-mini) for inline edits (CMD+K), formatting, and autocomplete.
4. **Chat Pruning:** If a chat exceeds 10 messages and the AI starts hallucinating or forgetting constraints, start a new chat and summarize the progress. 
5. **Enforce Isolation:** Ask for changes in specific function blocks, not full file rewrites. Example: *"Output only the modified `validate_token` function, do not rewrite the entire file."*

---

## 📂 Complete System Index

### 01. System Configuration
- [`cursor-settings.md`](./01-system/cursor-settings.md): Optimal IDE and AI settings.
- [`model-selection.md`](./01-system/model-selection.md): Strategy for choosing LLMs.
- [`token-management.md`](./01-system/token-management.md): Techniques to save tokens.
- [`prompting-methodology.md`](./01-system/prompting-methodology.md): The CTCF prompting framework.

### 02. Workflows
- [`project-start-workflow.md`](./02-workflows/project-start-workflow.md): Scaffolding new applications safely.
- [`debugging-workflow.md`](./02-workflows/debugging-workflow.md): Systematic error resolution.
- [`refactoring-workflow.md`](./02-workflows/refactoring-workflow.md): Safely updating legacy code.
- [`code-review-workflow.md`](./02-workflows/code-review-workflow.md): AI-assisted PR reviews.

### 03. Prompt Libraries
- [`architecture-prompts.md`](./03-prompts/architecture-prompts.md): System design and planning.
- [`frontend-prompts.md`](./03-prompts/frontend-prompts.md): React/Next.js components and UI.
- [`backend-prompts.md`](./03-prompts/backend-prompts.md): FastAPI, endpoints, and services.
- [`database-prompts.md`](./03-prompts/database-prompts.md): Schema design and optimizations.
- [`debugging-prompts.md`](./03-prompts/debugging-prompts.md): RCA and bug-fixing templates.

### 04. Context Templates
- [`project-context-template.md`](./04-context/project-context-template.md): The universal baseline file for all projects.

### 05. Rules & Boundaries
- [`ai-behavior-rules.md`](./05-rules/ai-behavior-rules.md): Core AI operational directives.
- [`frontend-rules.md`](./05-rules/frontend-rules.md): UI, state management, and aesthetic standards.
- [`backend-rules.md`](./05-rules/backend-rules.md): Layered architecture and API strictness.
- [`database-rules.md`](./05-rules/database-rules.md): Normalization, constraints, and safe migrations.
- [`security-rules.md`](./05-rules/security-rules.md): Hardening, RBAC, and vulnerability prevention.

### 06. Quality Checklists
- [`before-commit.md`](./06-checklists/before-commit.md): Pre-commit verification steps.
- [`before-deploy.md`](./06-checklists/before-deploy.md): Pre-production safety checks.

---
*Maintained for highly efficient, token-optimized AI-assisted software engineering.*