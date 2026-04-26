# 📖 BRDD Specification (The "Source of Truth")

This specification is the language-agnostic technical specification that defines the rules, contracts, and expectations for any BRDD-compliant implementation.

## 🏛 Core Rules & Service Boundaries
1. **UseCase Orchestration**: The `UseCase` exists exclusively to orchestrate the interaction between multiple services. It must not execute operations directly. It must always return an `ExecutionContext` encapsulating the `ValidationContext`, setters, effects, and return data.
2. **Simple Flow Bypass**: A `UseCase` is not needed (and should not be used) if only a single `ClientService` or `BusinessService` is called, as there is no interaction to orchestrate.
3. **Execution Delegation**: A `UseCase` must always delegate operations to a `BusinessService` or `ClientService`.
4. **Validation Isolation**: `ValidateService` executes validations. It must not fetch missing data; it requires pre-enriched parameters. It always returns a `ValidationContext`.
5. **Enrichment Isolation**: `EnrichService` is strictly used to query and fetch missing data to avoid mixing data gathering with validation or business logic.
6. **External Mapping**: `ClientService` handles all calls to external systems and is responsible for converting internal data to external data structures, and vice-versa.
7. **Unique Rule Coding**: Every business logic must have a unique identifier (e.g., R001, SET001).
8. **Unified Response Pattern**: Standardized API responses across all layers.

## 📂 Documentation Structure
To ensure scalability, technical documentation (Business Rules, Setters, and Effects) must be grouped under their respective **Use Cases**.

### Hierarchy:
- **Use Case**: The high-level business goal (e.g., UC001: Order Checkout).
    - **Business Rules (R)**: Validations that can block the process.
    - **Setters (SET)**: Internal state changes.
    - **External Effects (EXT)**: Interactions with third-party systems.

## 🤖 IA-Friendly
This specification is designed to be easily parsed by AI agents to ensure consistency across different language implementations.
