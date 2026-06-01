# Security & Hardening Rules

## Purpose
To establish absolute, non-negotiable security boundaries for all AI-generated code. These rules ensure the application is hardened against common vulnerabilities (OWASP Top 10) and maintains strict data privacy and access control standards.

## 1. Secrets & Environment Configuration
- **No Hardcoding:** Never hardcode passwords, API keys, JWT secrets, database connection strings, or cryptographic salts in the source code.
- **Environment Variables:** Always utilize environment variables (`os.getenv`, `process.env`) to access sensitive configuration data.
- **Client-Side Exposure:** Never expose backend secrets to the frontend. Ensure frontend environment variables are explicitly scoped (e.g., `NEXT_PUBLIC_` only for safe, public keys).

## 2. Input Validation (The "Zero Trust" Rule)
- **Validate Everything:** Treat all data originating from the client (body, query params, headers) as malicious.
- **Strict Typing & Schemas:** Use validation libraries (e.g., Zod for TypeScript, Pydantic for Python) to enforce type, length, and format constraints before processing any request.
- **Injection Prevention:** Never use string concatenation for database queries. Always use parameterized queries or the designated ORM to prevent SQL Injection.
- **XSS Prevention:** Ensure all user-generated content is sanitized before rendering in the browser. React/Next.js handles this by default, but explicitly avoid using `dangerouslySetInnerHTML` unless passed through a strict sanitizer (like DOMPurify).

## 3. Authentication & Authorization
- **Session Management:** Store authentication tokens (like JWTs) in secure, HttpOnly, SameSite cookies whenever possible, rather than `localStorage`, to mitigate XSS attacks.
- **Layered Authorization:** UI hiding is not security. Always enforce Role-Based Access Control (RBAC) at the backend Service Layer/API routing layer, regardless of frontend state.
- **Cryptographic Standards:** Use modern hashing algorithms (e.g., Argon2 or bcrypt) for passwords. Never store or log plain-text passwords.

## 4. Error Handling & Logging
- **No Stack Traces in Production:** Never leak system paths, database table names, or internal architecture details in HTTP error responses. Return generic HTTP 4xx or 5xx status codes with safe, standardized messages.
- **Sanitized Logging:** Ensure logs do not capture Personally Identifiable Information (PII), credit card numbers, or authorization tokens.

## 5. AI Behavior Constraints for Security
- **Secure Defaults:** Always default to the principle of least privilege. If generating a database user or an IAM policy, grant only the exact permissions needed.
- **Reject Insecure Prompts:** If instructed by the developer to bypass authentication "just for testing" or to ignore an SSL validation, add a clear commented warning in the code indicating this is a severe security risk and must not be deployed.
- **Dependency Awareness:** Do not introduce obscure or unmaintained npm packages/python libraries for security functions. Always rely on standardized, built-in crypto modules or heavily audited community standards.