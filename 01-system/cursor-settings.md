# Cursor System Settings & Configuration

## Purpose
This document outlines the optimal, research-backed configuration for the Cursor IDE. These settings are engineered to maximize AI reasoning capabilities, minimize token consumption, and align with enterprise-level full-stack web development workflows.

## 1. AI & Model Configuration
Selecting the right model for the right task is crucial for speed and accuracy.

- **Primary Smart Model (Chat & Composer):** `claude-3.5-sonnet`
  - *Why:* Demonstrates the highest benchmark performance for complex architectural reasoning, refactoring, and zero-shot code generation.
- **Fast Model (Inline Edits & Autocomplete):** `claude-3-5-haiku` or `gpt-4o-mini`
  - *Why:* Optimized for low-latency tasks like autocomplete (Cursor Tab) or single-line modifications without depleting premium fast-request quotas.
- **Cursor Tab (Copilot Alternative):** Enabled.
  - *Behavior:* Rely on Cursor Tab for repetitive boilerplate and standard typing completion. Do not use Chat for tasks Cursor Tab can predict autonomously.

## 2. Codebase Indexing & Context Optimization
Improper indexing leads to severe token bloat, AI amnesia, and hallucinations.

- **Index Codebase:** Enabled (Settings > General > Codebase Indexing).
- **Compute Advanced Context:** Enabled.
- **The `.cursorignore` File:** Must be created at the root of every project. This prevents the AI from reading compiled assets or external libraries, which pollutes the context window.
  ```text
  node_modules/
  dist/
  build/
  .next/
  out/
  coverage/
  *.log
  .env*
  .git/

```

## 3. Composer & Agent Mode Settings

Composer is the most powerful tool for multi-file generation, but it requires strict boundaries to prevent architectural drift.

* **Composer (CMD/CTRL + I):** Enabled.
* **Default View:** Use 'Full Screen' for Phase 3 (Scaffolding) of our project start workflow. Use 'Floating' for localized, single-file feature additions.
* **Agent Mode:** Disabled by default.
* *Why:* Autonomous execution without step-by-step verification often leads to cascading errors in large codebases. Only enable Agent Mode for strictly defined, isolated, and repetitive tasks (e.g., "Update all interface names in the `models` directory to use the `I` prefix").



## 4. Rules Binding & Global Prompting

Automate the integration of our AI engineering system.

* **Rules for AI (`.cursorrules`):**
Instead of pasting rules into every chat, copy the contents of `05-rules/ai-behavior-rules.md` into the project's local `.cursorrules` file. Cursor will automatically append these instructions to system prompts.
* **Auto-include Context:** Ensure the setting to automatically attach the current file's context is enabled, but actively prune unrelated files from the chat context window before submitting a prompt.

## 5. Essential IDE Extensions

Cursor is a fork of VS Code. Maintain these extensions to enforce code quality locally before asking the AI to review it.

* **Error Lens:** Instantly highlights syntax errors inline, preventing you from wasting AI tokens on obvious typos.
* **Prettier - Code formatter:** Automates formatting so the AI does not consume tokens adjusting indentation or spacing.
* **ESLint / Ruff:** Real-time linting for JavaScript/TypeScript and Python respectively.
* **Docker:** Essential for reviewing containerized environments natively.
* **GitLens:** Useful for checking the commit history before asking the AI to refactor legacy code.

## 6. Privacy & Security

For enterprise or proprietary codebases:

* **Privacy Mode:** Enabled (Settings > General > Privacy Mode).
* *Why:* Ensures your codebase, API keys, and prompts are not stored or used by third-party providers to train external foundational models.



## System Setup Verification Checklist

* [ ] Primary and Fast models are correctly assigned in settings.
* [ ] `.cursorignore` is configured and active in the project root.
* [ ] `ai-behavior-rules.md` logic is loaded into `.cursorrules`.
* [ ] Privacy Mode is activated.
* [ ] Code formatting extensions are installed and set as the default formatters.
