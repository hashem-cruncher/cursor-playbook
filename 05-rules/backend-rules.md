# Backend Development Rules

## Purpose
To establish non-negotiable standards for backend code generation, focusing on security, high performance, clean architecture, and container-ready deployments.

## 1. Clean Architecture (Layered Design)
- **Separation of Concerns:** Strictly enforce the division between Routing (API endpoints), Business Logic (Service layer), and Data Access (Repositories/Models).
- **Routers/Controllers:** API endpoints must only handle HTTP request/response parsing and route delegation. No business logic or direct database queries should exist here.
- **Service Layer:** All core business rules and transformations belong in dedicated service classes or functions.

## 2. API Design & Validation (FastAPI/Python Context)
- **Data Validation:** Validate all incoming requests at the boundary layer using strict schemas (e.g., Pydantic). Never trust client input.
- **Response Formatting:** Standardize API responses. Success and error payloads must follow a consistent JSON structure across all endpoints.
- **Documentation:** Ensure routes are self-documenting. Use explicit type hints and route decorators that automatically generate OpenAPI/Swagger documentation.

## 3. Database Interactions
- **ORM Usage:** Use the designated ORM (e.g., SQLAlchemy) for all database operations. 
- **Asynchronous Queries:** Use async database drivers where supported to prevent blocking the event loop during heavy I/O operations.
- **Query Optimization:** Prevent N+1 query problems by using explicit eager loading for relational data. Avoid pulling entire tables into memory; use pagination and filtering at the database level.

## 4. Security & Environment Configuration
- **Secrets Management:** Never hardcode API keys, database URLs, or JWT secrets. Always read from environment variables (`os.getenv`).
- **Error Handling:** Catch exceptions globally. Never leak internal stack traces to the client in production environments. Return generic, safe error messages accompanied by appropriate HTTP status codes.

## 5. DevOps & MLOps Alignment
- **Containerization Readiness:** Write code assuming it will run in isolated Docker containers (e.g., ephemeral filesystems, stateless execution).
- **Logging:** Implement structured logging (JSON format preferred). Include context like request IDs or user IDs to facilitate debugging in distributed systems.
- **Statelessness:** Ensure backend nodes are stateless to allow horizontal scaling (e.g., via Azure App Services or Kubernetes). Store sessions and temporary states in caching layers like Redis.