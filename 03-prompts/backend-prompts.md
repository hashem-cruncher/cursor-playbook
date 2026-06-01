# Backend & API Component Prompts

## Purpose
A highly structured prompt library for generating robust backend architecture, API endpoints, and business logic layers. These prompts enforce clean architecture, strict data validation, and asynchronous performance optimizations.

## When To Use
- Creating new API endpoints and routers.
- Building isolated service classes for business logic.
- Implementing middleware, authentication, or authorization guards.
- Writing background tasks or integration layers for MLOps pipelines.

---

## 1. The Clean API Endpoint Prompt
*Use this to generate a secure, well-documented endpoint that delegates logic to a service layer.*

**Prompt Template:**
```text
Task: Create a new REST API endpoint for [Insert Domain, e.g., processing data forecasting requests].

Context:
- Review @project-context.md for backend framework specifications.
- Schema reference: @[Pydantic_or_DTO_Schema_File].

Requirements:
1. Routing Layer Only: Build the route handler. Do not put business logic inside the route function.
2. Delegation: Inject and call a dedicated method from the @[Service_Layer_File].
3. Validation: Use strict input validation via our schema definitions.
4. Error Handling: Catch exceptions thrown by the service layer and translate them into standardized HTTP responses (e.g., 400 for bad input, 404 for not found). Do not leak system paths.
5. Documentation: Include explicit return types, response models, and summary docstrings for OpenAPI/Swagger generation.

Constraint: Output only the routing function and necessary imports.

```

---

## 2. The Service Layer (Business Logic) Prompt

*Use this to encapsulate core logic, ensuring it remains isolated from HTTP requests or direct database sessions.*

**Prompt Template:**

```text
Task: Implement the core business logic for [Insert Action, e.g., validating and aggregating user metrics] within the Service Layer.

Context:
- File to modify: @[Target_Service_File].
- Required Data Models: @[Relevant_Models_File].

Requirements:
1. Isolation: The function must accept pure data structures (dicts, dataclasses, or validated schemas), not raw HTTP request objects.
2. Modularity: Break the logic into small, testable private helper methods within the service class if it exceeds 30 lines.
3. Database Abstraction: Do not write raw SQL or direct DB commits here. Call the appropriate methods from the Repository/Data Access layer.
4. Concurrency: Use async/await paradigms strictly if this operation involves I/O blocking calls (network requests, DB reads).

Constraint: Maintain strict typing for all arguments and return values.

```

---

## 3. The Middleware & Security Guard Prompt

*Use this to implement system-wide intercepts like JWT validation, rate limiting, or logging.*

**Prompt Template:**

```text
Task: Implement a middleware/dependency function for [Insert Purpose, e.g., JWT Token verification and Role-Based Access Control].

Requirements:
1. Security Focus: Read the secret key and algorithm configurations securely from environment variables, never hardcoded.
2. Performance: Ensure the intercept is highly optimized. Use caching (e.g., Redis lookups) if validating sessions against a database to prevent latency.
3. Failure State: If validation fails, immediately raise an unauthorized/forbidden exception with a generic message. Do not specify whether the token was expired vs. malformed to the client.

Constraint: Output the middleware implementation and demonstrate how to inject it into a sample route.

```

---

## 4. The Background Task / MLOps Integration Prompt

*Use this for heavy computations, model inferences, or asynchronous job processing.*

**Prompt Template:**

```text
Task: Create a background worker task for [Insert Job, e.g., running the predictive forecasting model on newly uploaded datasets].

Context:
- This task will run asynchronously outside the main request-response cycle.

Requirements:
1. Execution: Implement the task using our standard background job processor [e.g., Celery, FastAPI BackgroundTasks, or Azure Functions].
2. State Management: The task must update the job status (e.g., 'processing', 'completed', 'failed') in the database so the client can poll the status.
3. Resilience: Include a try/except block that catches computational failures, logs the traceback internally, and safely marks the job as 'failed' without crashing the worker node.
4. Container Readiness: Ensure file paths for temporary data use cross-platform libraries (e.g., `pathlib`) and assume an ephemeral filesystem.

Constraint: Output the task function and the corresponding trigger mechanism.

```

---

## Best Practices for Backend Prompts

* **Enforce Separation:** Always explicitly ban the AI from writing SQL queries inside API route files.
* **Strict Data Contracts:** Always reference the exact schema file `@schemas.py`. If the AI guesses the payload structure, the endpoint will fail during client integration.
* **Audit Imports:** AI models occasionally import synchronous libraries in asynchronous environments. Always verify that generated I/O operations are non-blocking.