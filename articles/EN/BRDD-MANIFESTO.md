# 📜 BRDD Manifesto: The 4 Golden Rules

**Business Rule Driven Design (BRDD)** is not just a folder structure; it is a technical commitment grounded in four non-negotiable pillars. The goal is to ensure that Business and Code operate under the same grammar.

## 🏛 The 4 Pillars

### 1. Unique Rule Coding
Every business rule, validation, or side effect must have a unique identifier code (e.g., `R001`). In BRDD, the code is not just logic; it is the direct link to the business documentation. If a rule changes on paper, the developer knows exactly which line of code must be adjusted.

### 2. Execution Context Narrative
Use Cases should not return raw data; they should narrate what happened during the process through the `ExecutionContext`. The system output becomes an execution report: what was validated, what was changed, and which external effects were triggered. This drastically simplifies debugging and process auditing.

### 3. Service Specialization
To maintain maintainability, logic must be divided into clear semantic roles, avoiding services with overloaded responsibilities. In BRDD, each service has a defined purpose:
*   **UseCase:** Orchestrator of the logical flow.
*   **ValidateService:** Specialist in validating the integrity of a rule.
*   **EnrichService:** Responsible for preparing the data context.
*   **Client/Listener:** Responsible for integration with external domains.

### 4. Unified Response Pattern
Every API interaction must follow a strict and predictable JSON format. This ensures that any consumer—be it a user interface or an AI agent—knows exactly how to interpret the success or failure of an operation.

---

## 🤖 AI-First Architecture
BRDD was specifically designed to maximize the efficiency of AI and Copilots. By isolating rules into metadata and specialized services, we eliminate technical ambiguity.

- **Master Prompts:** Documentation in `.md` acts as a "live" and executable technical specification.
- **Architectural Security:** With well-defined boundaries, we prevent AI from introducing unintended side effects during the implementation of new validations.

---

### 🔗 Next Steps
See a real example in the **[Practical Example](./BRDD-PRACTICAL-EXAMPLE.md)**.
Understand the origin of this pattern in **[The Journey](./BRDD-STORY.md)**.