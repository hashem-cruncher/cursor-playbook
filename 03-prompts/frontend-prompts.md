# Frontend Component & API Integration Prompts

## Purpose
A specialized prompt library for generating modular frontend components, handling complex UI states, and integrating backend APIs seamlessly. These prompts enforce strict TypeScript typing, minimalistic design paradigms, and optimal state boundaries.

## When To Use
- Building new UI components (functional, presentation, or layout).
- Creating data-fetching layers and state hooks.
- Connecting frontend components to backend endpoints (FastAPI/REST contracts).
- Implementing user input validation and dynamic UI feedback.

---

## 1. The Clean UI Component Scaffold Prompt
*Use this in Composer or Chat to build an enterprise-grade, accessible, and responsive component.*

**Prompt Template:**
```text
Role: Act as a Senior Frontend Engineer.
Task: Scaffold a reusable React component named [ComponentName].

Context:
- Review @project-context.md for frontend stack constraints.
- Refer to @frontend-rules.md for styling and behavioral guidelines.

Requirements:
1. Design: Apply a minimalistic, enterprise-grade aesthetic. Use standard Tailwind utility classes. Focus on clean typography, precise padding, and consistent grid/flex structures.
2. Architecture: Make it self-contained and modular. Elevate interactive configurations to Component Props using strict TypeScript definitions. Do not use 'any'.
3. Accessibility (a11y): Include essential ARIA attributes, semantic HTML elements, and handle focus/keyboard navigation where interactive.
4. Edge Cases: Include visual states for: Empty, Loading, and Error/Validation failures.

Constraint: Output only the code for the component and its types. Do not write inline styles. Do not include external state stores unless requested.

```

---

## 2. The Strict API Integration & Fetching Prompt

*Use this to bind a UI component to backend data-fetching mechanisms efficiently.*

**Prompt Template:**

```text
Task: Integrate backend API connectivity into the [ComponentName] component.

Context:
- Review the expected API payload schema in @[Schema_or_Backend_Model_File].
- Target Endpoint: [Method] [Endpoint Path, e.g., GET /api/v1/metrics/summary]
- Data Fetching Strategy: Use [e.g., React Query / native fetch with hooks] as defined in our stack.

Requirements:
1. Data Fetching: Build a custom hook or fetching function to query the specified endpoint. Wrap types securely around the request and response models.
2. State Separation: Separate the server state logic from the presentation component. Ensure the presentation layer receives structured, predictable data models.
3. Robust Error Handling: Capture HTTP error states (4xx, 5xx) globally from the backend payload. Safely expose readable feedback messages to the user view without crashing the component tree.
4. Optimization: Implement basic request cancellation or caching configurations to optimize network consumption.

Constraint: Do not create local state variables to duplicate server data. Follow our layered architecture guidelines.

```

---

## 3. The Enterprise Form & Validation Prompt

*Use this to generate complex input forms with strict client-side verification before payload submission.*

**Prompt Template:**

```text
Task: Build a secure, structured input form for [Insert Purpose, e.g., Enterprise User Onboarding].

Requirements:
1. Inputs Needed: [List fields, e.g., full_name, email, organization_id, tier].
2. Validation Rules:
   - [e.g., email must match enterprise domain regex].
   - [e.g., organization_id is a mandatory uuid].
3. Execution: Implement real-time client-side validation using [e.g., Zod schemas or basic controlled inputs with pure functions].
4. UI Feedback: Show precise inline validation error warnings immediately below the failing input boundary. Disable the submit trigger button while loading or while fields contain active errors.

Constraint: Ensure strict type alignment between the local form schema data and the final payload sent to the API.

```

---

## 4. The Responsive Layout & Dark Aesthetic Refinement Prompt

*Use this when your layout is functional but needs polishing to match a premium high-tech look.*

**Prompt Template:**

```text
Task: Refine the aesthetic layout of the current component in @[File_Name].

Context:
- Current layout feels flat or unoptimized for dark/enterprise visual styling.

Requirements:
1. Design Enhancement: Incorporate a premium dark aesthetic. Apply deep background color gradients, precise border treatments for micro-separations, and subtle depth shadows.
2. Responsiveness: Audit and refactor flex/grid layers. Ensure smooth shifting from single-column mobile views to multi-column desktop screen configurations (`sm:`, `md:`, `lg:` boundaries).
3. Performance Check: Eliminate excessive class nesting or overlapping layout utility definitions.

Constraint: Do not touch the core logic, handlers, or state operations within the file. Focus exclusively on Tailwind class updates and presentation shell enhancements.

```

---

## Best Practices for Frontend Prompts

* **Isolate Logic:** Never prompt for the layout and the heavy API integration logic simultaneously in one huge execution block. Generate the presentation shell first, test it, then hook up the data pipelines.
* **Mock Data First:** If the backend is not ready, instruct the AI: "Provide an isolated static object containing realistic mock data adhering to the typescript interface so we can test the visual layer independently."
* **Prune Tailwind:** AI models love stacking Tailwind classes haphazardly. Run a visual check to confirm the AI isn't duplicating width/height constraints or border settings within the same node string.
