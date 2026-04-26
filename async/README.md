# ⚡ BRDD Async (Beta)

**Extensions for asynchronous flows, messaging, and event-driven orchestration.**

BRDD Async extends the core pattern to handle processes that span across multiple time-frames, message queues, and background workers. It introduces the `FlowContext` to maintain state and traceability across distributed steps.

## 🎯 Key Concepts

*   **FlowEngineService:** Orchestrates the interaction of services in each asynchronous step.
*   **FlowContextLoader:** Loads the current state of the flow (executed steps, next steps, prerequisites).
*   **FlowContextAccumulator:** Persists the state after each step execution.
*   **FlowContext:** The narrative carrier for asynchronous state.

## 🚀 Status: Beta
This extension is currently being formalized. Implementations for Python and .NET are in the draft phase.

## 📚 Documentation
- [Draft Description](./draft/desc.md)
- [Main BRDD Manifesto](../BRDD.md)

---
Officially maintained by **Defol Tech**.
