# Frontend Development Rules

## Purpose
To enforce strict guidelines for frontend code generation. These rules ensure a highly modular, performant, and visually consistent user interface that adheres to enterprise-grade standards.

## 1. Architectural Patterns
- **Component Modularity:** Build small, single-responsibility components. Avoid monolithic files. 
- **Server vs. Client Components (Next.js):** Default to Server Components for data fetching and SEO optimization. Only use the `"use client"` directive when interactivity (hooks, event listeners) is strictly required.
- **Directory Structure:** Keep UI components, business logic (hooks/utils), and state management strictly separated in their respective directories.

## 2. UI/UX & Aesthetics
- **Design Language:** Adhere to an enterprise-grade, minimalistic, and futuristic aesthetic. 
- **Styling:** Use standard Tailwind CSS utility classes exclusively. 
- **Lighting & Color:** Utilize dark gradients, deep backgrounds, and cinematic lighting effects for depth where appropriate. Incorporate subtle geometric or "Arabic-tech" design influences logically, avoiding cluttered visuals.
- **Anti-Pattern:** Never use inline styles (`style={{ ... }}`). Do not invent custom CSS classes unless handling complex animations that Tailwind cannot support natively.

## 3. State Management & Data Flow
- **Local State:** Use `useState` and `useReducer` for component-level state.
- **Server State:** Use robust data-fetching libraries (e.g., React Query or Next.js native fetch with caching) instead of manually managing loading/error states with `useEffect`.
- **Global State:** Only elevate state to global stores (e.g., Zustand, Context API) if the data is shared across widely separated component trees.

## 4. Typing and Interfaces
- **Strict TypeScript:** All props, state, and API responses must be strongly typed. 
- **No 'any':** The use of the `any` type is strictly forbidden. Use `unknown` if the type is truly dynamic, followed by type narrowing.

## 5. AI Behavior Constraints for Frontend
- **Do Not Guess Assets:** If an image or icon is needed, use standard placeholder components (e.g., Lucide React icons) rather than hallucinating generic URL paths.
- **Accessibility (a11y):** Always include `aria-labels`, `alt` tags, and ensure proper keyboard navigation semantics for interactive elements.