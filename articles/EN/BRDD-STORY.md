<div align="center">
  <img src="../../assets/generic_cover_landscape.png" alt="BRDD Story Cover" width="800" />
</div>

# BRDD - BUSINESS RULE DRIVEN DESIGN (The Journey)

Working across various business sectors in the IT world, I gained knowledge by comparing many different realities. At the end of the day, I realized that structural problems are almost always the same: a chronic misunderstanding between Product/Business teams and Development teams; poorly documented projects; and complex legacy systems that only their creators can maintain. Even the use of AI presents difficulties in these contexts, as it requires high token consumption trying to "guess" the intent behind poorly structured code, which harms the performance of new team members and the evolution of the project.

Aiming to eliminate this gap between Business and IT, and to organize code without creating unnecessary layers of complexity (which often don't pay off due to high learning curves), I developed a design pattern strictly based on **business rules**. This design was not born ready; it was refined over years, with each new project started from scratch, until it was tested in high-complexity scenarios and proved its effectiveness.

### Model Foundations
I adopted **MVC** and **DDD** as the foundation due to their simplicity and high market adoption. DDD allows the model to work well for both large monoliths and microservices. Since this model respects widely accepted standards, it can be adapted to the recommended practices for any language or framework, requiring only fine-tuning without violating fundamental rules.

---

## Roles and Responsibilities

When analyzing different functionalities in distinct systems, it becomes clear that, in an abstract way, the vast majority of features follow a common logical flow:

1. **Enrichment of input data** (context preparation);
2. **Validation of business rules** (integrity guarantee);
3. **Execution of the business rule** (main action);
4. **Enrichment of the response** (output formatting).

Although the order may vary or some steps may be optional, the flow invariably contains: **Enrichment**, **Validation**, and **Execution**. 

Usually, these actions are mixed within the *Service* layer because there is no rigid market consensus on where to draw these boundaries. In the **BRDD** model, I make it explicit that this separation should be made into specialized service types:

*   **Service:** Solely responsible for executing the main business logic.
*   **ValidationService:** Single-responsibility classes focused on validating a specific rule (e.g., `CheckStockValidator`).
*   **EnrichService:** Classes focused on fetching or calculating data needed for the context (e.g., `GetProductInfoEnricher`).
*   **ClientService:** Responsible for communication with external systems.
*   **Listener:** Responsible for capturing external interactions and triggering flows.
*   **UseCase:** The orchestrator that uses `ValidationServices` and `EnrichServices` to compose the end-to-end flow.

---

## The Business Context

Any validated rule, system-defined value, or side effect must be identified and explicitly assigned to a code representing its origin in the business context. This origin must have a unique identifier and be documented in the repository (the use of `.md` files, such as `BUSINESS_RULES.md`, is recommended).

### Example Flow
To perform product consumption, one must first validate if there is sufficient stock. After consumption, partner systems must be notified, and the update date must be recorded.

In this scenario, we map:
*   **Business Rule:** `(R001)` Validate stock availability.
*   **Use Case:** `(US001)` Product consumption.
*   **Side Effect:** `(CE001)` External system notification.
*   **Loaded Value:** `(SET001)` Operation timestamp.

This way, when the business team defines a new feature, they already do so using codes that the developer will instantly recognize in the code.

---

## AI-First Architecture

The great differentiator of BRDD today is its native support for AI-assisted development. By making the code semantically identical to the documentation, we create an environment where **AI Agents** can act as strategic partners, understanding the "why" behind every line. This not only accelerates development but ensures the architecture remains intact and aligned with business goals as the system scales.

---

### 🔗 Next Steps
For a lean formalization of the rules, read the **[BRDD Manifesto](../../BRDD.md)**.
Check out the repository on [GitHub](https://github.com/brdd-design/brdd).