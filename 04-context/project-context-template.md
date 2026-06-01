# 🏗️ Project Context & Architecture



> **AI INSTRUCTION:** Read this file carefully before suggesting any architectural changes, writing new files, or refactoring existing code. This file acts as the single source of truth for the project's technical direction.



## 1. Project Overview

- **Name:** [Insert Project Name]

- **Description:** [Insert 1-2 sentences describing the core purpose of the project. e.g., A centralized platform for monitoring KPIs and performance metrics.]

- **Primary Audience:** [e.g., Enterprise users, Internal management]



## 2. Tech Stack & Versions

*Do not suggest libraries or syntaxes incompatible with these versions.*



- **Frontend:** [e.g., Next.js 14 (App Router), React 18]

- **Styling/UI:** [e.g., TailwindCSS, Minimalistic/Enterprise-grade design system]

- **Backend/API:** [e.g., Python 3.11, FastAPI]

- **Database:** [e.g., PostgreSQL, Redis for caching]

- **Infrastructure/DevOps:** [e.g., Docker, Azure, GitHub Actions]



## 📐 3. Core Architecture

- **Pattern:** [e.g., Client-Server, Microservices, Monorepo]

- **State Management:** [e.g., Zustand for global state, React Query for server state]

- **API Communication:** [e.g., RESTful APIs with strict Pydantic validation]

- **Authentication:** [e.g., JWT tokens, OAuth2]



## 📂 4. Expected Folder Structure

*When creating new files, strictly adhere to this structure:*



```text

[Insert your standard structure, e.g.:]

/frontend

  /src

    /components    # Reusable UI components

    /features      # Domain-specific logic

    /services      # API calls and external integrations

/backend

  /app

    /api           # FastAPI routes/endpoints

    /core          # Config, security, database setup

    /models        # Database models

    /services      # Business logic

```



## 📜 5. Crucial Coding Standards



* **Naming Conventions:** - Frontend: `camelCase` for variables, `PascalCase` for components.

* Backend: `snake_case` for variables/functions (Python standard), `PascalCase` for Classes.





* **Error Handling:** All backend endpoints must return standardized JSON error responses. Frontend must handle loading/error states gracefully.

* **Testing:** [e.g., Write unit tests for business logic using Pytest before integration.]



## 🚀 6. Current Phase / Focus



> **[Update this section as the project evolves]**



* **Current Goal:** [e.g., Implementing the authentication flow and user roles.]

* **Known Technical Debt:** [e.g., Need to optimize the reporting database queries later.]