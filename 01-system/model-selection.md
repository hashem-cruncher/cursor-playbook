# Model Selection & Interaction Strategy

## Purpose
To define a systematic approach for selecting the appropriate Large Language Model (LLM) and Cursor interaction mode. Choosing the correct combination prevents token waste, reduces latency, and ensures high architectural accuracy for complex tasks.

## 1. Model Categories & Usage

### Smart Models (High Reasoning / High Cost)
**Models:** Claude 3.5 Sonnet, GPT-4o, o1-preview/o1-mini.
**Characteristics:** High contextual awareness, excellent at zero-shot architecture, deep refactoring, and complex debugging.
**Strict Use Cases:**
- Architectural planning and system design.
- Multi-file refactoring or scaffolding (Composer Phase).
- Resolving complex logical bugs or memory leaks.
- Initializing a new feature workflow.

### Fast Models (Low Latency / Low Cost)
**Models:** Claude 3.5 Haiku, GPT-4o-mini, Cursor-Small.
**Characteristics:** Extremely fast response times, smaller context windows, prone to hallucination on complex logic.
**Strict Use Cases:**
- Inline edits (CMD/CTRL + K) for single-line changes.
- Writing boilerplate code (e.g., getters/setters, basic interfaces).
- Fixing obvious syntax errors or typos (linting fixes).
- Writing basic unit tests for pure functions.
- Autocomplete (Cursor Tab).

## 2. Cursor Interaction Modes

### Inline Edit (CMD/CTRL + K)
- **Primary Model:** Fast Model (e.g., Haiku).
- **Scope:** Single file, specific highlighted block.
- **Rule:** Use for immediate, isolated modifications. Do not use for cross-file logic changes.

### Chat Mode (CMD/CTRL + L)
- **Primary Model:** Smart Model (e.g., Sonnet).
- **Scope:** 1 to 3 files.
- **Rule:** Use for Root Cause Analysis (RCA), architectural discussions, and planning. Treat Chat as the "Architect" and restrict it from writing large blocks of final code until a plan is agreed upon.

### Composer (CMD/CTRL + I)
- **Primary Model:** Smart Model (e.g., Sonnet).
- **Scope:** Multi-file scaffolding and feature implementation.
- **Rule:** Only invoke Composer *after* an architectural plan has been approved in Chat. Use the `Floating` mode for minor feature additions and `Full Screen` for major structural scaffolding.

### Agent Mode (Autonomous Execution)
- **Primary Model:** Smart Model (e.g., Sonnet).
- **Scope:** Broad codebase changes.
- **Rule:** Restrict Agent Mode usage. Only enable it for deterministic, highly repetitive tasks where the start and end states are explicit (e.g., "Find all instances of `any` in the `interfaces` folder and replace them with strict types based on the backend schema"). Never use Agent Mode for open-ended creative tasks.

## 3. Decision Matrix

| Task Complexity | Recommended Model | Recommended Mode | Token Consumption Risk |
| :--- | :--- | :--- | :--- |
| Syntax Fix / Typo | Fast Model | Inline (CMD+K) | Low |
| Boilerplate / Typings | Fast Model | Inline (CMD+K) | Low |
| Root Cause Analysis | Smart Model | Chat (CMD+L) | Medium |
| System Design / Planning | Smart Model | Chat (CMD+L) | Medium |
| Single Feature Logic | Smart Model | Composer (Floating) | High |
| Multi-file Scaffolding | Smart Model | Composer (Full Screen) | Very High |
| Global Variable Rename | Smart Model | Agent Mode | Extreme |

## 4. Token & Cost Optimization Strategies
- **The "Downgrade" Rule:** Once a complex problem is solved in Chat using a Smart Model, and you only need to apply the resulting 5-line fix, switch to Inline Edit with a Fast Model to apply it.
- **Context Pruning:** Before asking a Smart Model to generate a complex Composer output, manually remove any referenced files from the context window that are no longer strictly necessary for the current step.
- **Avoid "Lazy" Prompts with Smart Models:** Using a premium model to ask "Fix the red squiggly line in this file" wastes valuable reasoning compute. Use Fast Models for linting issues.