# AI Behavior & Code Generation Rules

##  Purpose
This document defines the absolute, non-negotiable rules for Cursor AI behavior, code generation, and architectural decisions. It is designed to minimize token waste, prevent hallucinations, and maintain an enterprise-grade, minimalistic, and highly scalable codebase.

##  1. Core AI Directives (Zero-Tolerance Rules)
- **Do Not Guess:** If context is missing, stop and ask the developer. Never assume technical requirements or business logic.
- **Scope Containment:** NEVER modify files outside the direct scope of the current task. 
- **Token Economy:** Do not repeat the entire file in the output if only a few lines changed. Use precise search/replace blocks or specify the exact lines to modify.
- **Silent Execution:** Minimize conversational filler. Output code, architectural decisions, and required explanations directly. Skip greetings or unnecessary apologies.

##  2. Architectural & Engineering Standards
- **Simplicity Over Complexity:** Always prefer the most readable and straightforward solution. Avoid overengineering.
- **Modularity:** Keep functions and components small and focused on a single responsibility (SOLID principles).
- **Containerization Readiness:** Write 
- code assuming it will be containerized (e.g., Dockerized environments). Ensure environment variables are used for all configurations.
- **Clean Architecture:** Strictly separate concerns. UI components must not contain complex business logic or direct database queries.

##  3. Domain-Specific Rules

###  Frontend Rules (Enterprise & Minimalistic)
- **Component Design:** Build reusable, self-contained UI components. 
- **Styling:** Adhere to a minimalistic, professional design system. Avoid inline styles; use utility classes (e.g., Tailwind) or CSS modules consistently.
- **State Management:** Keep local state local. Only elevate state to global stores when absolutely necessary to prevent render-cycle bloat.
- **Types/Interfaces:** Enforce strict typing. Do not use `any`.

###  Backend & API Rules (Robust & Scalable)
- **Layered Structure:** Use standard routing (e.g., FastAPI/Express) -> Service Layer (Business Logic) -> Data Access Layer.
- **Validation:** Validate all incoming requests at the boundary layer before processing.
- **Error Handling:** Never swallow errors. Use structured, consistent error responses with appropriate HTTP status codes. Log errors comprehensively without exposing internal system paths to the client.
- **Performance:** Optimize database queries. Use pagination for large data sets and implement caching strategies where appropriate.

##  4. Anti-Hallucination & Token Optimization Techniques
- **Context Priming:** Always read the `@project-context.md` before suggesting major architectural shifts.
- **Incremental Implementation:** For large tasks, propose a phase-by-phase implementation plan first. Wait for developer approval before generating massive blocks of code.
- **Self-Correction:** Before outputting code, run a silent internal check: *Does this break existing imports? Is it compatible with the defined tech stack?*

##  Bad vs.  Good AI Responses

### Bad (Token Wasting & Risky)
> "Sure! Here is the entire 500-line `user_service.py` file rewritten with your one-line change. I also added a new library `xyz` that might help, and I changed the routing structure."

### Good (Precise & Professional)
> "To implement the new validation requirement, update the `UserService` class in `user_service.py`. 
> 
> ```python
> # ... existing code ...
> def validate_user(self, data: dict):
>     # Added strict email validation
>     if not re.match(EMAIL_REGEX, data.get("email", "")):
>         raise ValidationError("Invalid enterprise email format")
> # ... existing code ...
> ```
> No other files require modification."