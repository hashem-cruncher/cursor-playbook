# Architecture & Planning Prompts

## Purpose
This file contains production-grade prompts designed exclusively for architectural planning, system design, and large-scale refactoring. The goal is to force the AI into an "Architect Role" to evaluate trade-offs, generate diagrams, and create step-by-step implementation plans before any code is generated.

## When To Use
- Starting a new microservice or module.
- Designing database schemas and relations.
- Planning a complex state-management flow.
- Evaluating architectural trade-offs (e.g., SSR vs. CSR, SQL vs. NoSQL).

---

## 1. The "Plan Before Code" Prompt (System Design)
*Use this in Chat Mode (with a smart model like Claude 3.5 Sonnet) before using Composer.*

**Prompt Template:**
```text
Role: Act as a Staff Software Architect.
Task: I need to design the architecture for [Insert Feature/System Name, e.g., an RBAC authentication system].

Context: 
- Review @project-context.md for our current tech stack.
- Review @[Relevant_Existing_File] to understand how we currently handle similar data.

Requirements:
1. Propose 2 different architectural approaches to solve this.
2. For each approach, list the Pros, Cons, and Time Complexity / Scalability considerations.
3. Recommend the best approach based on our tech stack.
4. Generate a Mermaid.js diagram illustrating the data flow of your recommended approach.

Constraint: Do NOT write any application code. Output only the analysis, the recommendation, and the diagram. Wait for my approval before proceeding to implementation.

```

---

## 2. The Database Schema Design Prompt

*Use this to map out models, relations, and indexes safely without breaking existing schemas.*

**Prompt Template:**

```text
Task: Design the database schema for [Insert Domain, e.g., the new Invoice Management module].

Context:
- Database type: [e.g., PostgreSQL]
- ORM/Query Builder: [e.g., Prisma / SQLAlchemy]
- Review our current schema at @schema.prisma or @models/.

Requirements:
1. Define the necessary tables/entities.
2. Explicitly define Foreign Keys and indexing strategies for high-performance querying.
3. List the data types and strict constraints (e.g., NOT NULL, UNIQUE) for each column.
4. Identify any potential data anomalies or migration risks with existing tables.

Constraint: Output the schema definition in the format of our ORM. Do not modify existing core tables unless explicitly stated.

```

---

## 3. The API Contract & Payload Design Prompt

*Use this to agree on API endpoints between Frontend and Backend before implementation.*

**Prompt Template:**

```text
Task: Design the RESTful API contract for [Insert Feature, e.g., User Onboarding Flow].

Requirements:
1. List all required endpoints (Method, Path).
2. For each endpoint, provide the exact JSON request payload structure and types.
3. For each endpoint, provide the exact JSON response payload (Success and Error states with HTTP status codes).
4. Follow strict REST conventions and our standards outlined in @ai-behavior-rules.md.

Format: Use a Markdown table for the endpoint overview, followed by JSON code blocks for the payloads. Do not write the actual route handlers yet.

```

---

## 4. The Large-Scale Refactor Strategy Prompt

*Use this when tackling technical debt across multiple files.*

**Prompt Template:**

```text
Task: Plan a safe refactoring strategy for [Insert Module/Directory, e.g., the legacy Payment Gateway integration].

Context:
- The current code in @[Folder_Name] is highly coupled and lacks unit tests.

Requirements:
1. Analyze the current dependencies and pain points.
2. Break down the refactoring process into 3-4 isolated, testable phases.
3. Identify which phase carries the highest risk of breaking the production build.
4. Suggest what unit tests must be written *before* we start modifying the logic.

Constraint: Plan only. Detail the sequential steps we will take. Do not generate the refactored code yet.

```

---

## Best Practices for Architectural Prompting

* **Non-Goals:** Always explicitly state what the AI should *not* do. (e.g., "Non-goal: Do not change the UI layer or CSS").
* **Sequence is Key:** Never accept a 10-step plan and say "Go do it all". Tell the AI: "Great plan. Let's execute Step 1 only. Let me know when you are done."
* **Mermaid Diagrams:** Cursor can render Mermaid charts directly in the chat. Visualizing data flow early prevents massive logical errors later.