<div align="center">
  <img src="../../assets/generic_cover_portrait.png" alt="BRDD Practical Example" width="800" />
</div>

# 🚀 BRDD in Practice: Implementation Example

In this article, we apply BRDD concepts to a real-world inventory management scenario to demonstrate how role separation simplifies architecture and makes code self-documenting.

> [!TIP]
> The complete code and folder structure for this example can be found in the [repositories/brdd-examples](../../repositories/brdd-examples) directory.

---

## Scenario: Assignment Submission

For this more complete example, let's imagine an education platform where a student is submitting an assignment. We need to ensure they are enrolled, the course is active, and the deadline hasn't passed.

### 1. Rule Mapping (BUSINESS_RULES.md)

First, we define the identifiers that connect the business to the implementation:

*   **UseCase:** `(UC_EDU_001)` Assignment Submission.
*   **Rule:** `(R001)` Validate if the submission is before the cut-off date.
*   **Rule:** `(R002)` Validate if the student has an active enrollment in the course.
*   **Effect:** `(EFF_DEACTIVATE)` [PRE] Deactivate previous submissions.
*   **Effect:** `(EFF_NOTIFY)` [POST] Notify instructors about the new submission.
*   **Setter:** `(SET_ID)` Assign a unique UUID to the submission.
*   **Setter:** `(SET_LATE)` Mark as late if after the `dueDate`.

### 2. UseCase Implementation (Full Orchestration)

The `UseCase` acts as the flow orchestrator, without needing to know the internal implementation of validations or data fetching. In BRDD, we prioritize single-responsibility services. Instead of a generic and bloated validator, we create specialized classes for each business rule.

```python
class SubmitAssignmentUseCase(UseCase):
    def __init__(self, 
                 enrich_service: SubmissionEnrichService,
                 validate_service: SubmissionValidateService,
                 business_service: SubmissionBusinessService):
        self.enrich = enrich_service
        self.validate = validate_service
        self.business = business_service

    def execute(self, input_data: SubmitInput) -> ExecutionContext:
        # 1. Initialize context for Use Case UC_EDU_001
        context = DefaultExecutionContext(input_data)
        
        # 2. Enrichment: Fetch Enrollment, Course, and Activity data
        data = self.enrich.enrich(context, input_data)
        
        # 3. Validation: Check R001, R002, R003...
        self.validate.validate(context, data)
        
        # If any rule fails (e.g., student not enrolled), the flow stops here
        if not context.is_valid():
            return context
            
        # 4. PRE Effect: Clear old submissions before saving the new one
        context.add_effect("EFF_DEACTIVATE", lambda: self.business.clear_old(data))
        
        # 5. Main Execution: Save the new assignment
        result = self.business.execute(data)
        
        # 6. Recording Setters and POST Effect
        context.set("SET_ID", result.id)
        context.set("SET_LATE", result.is_late)
        context.add_effect("EFF_NOTIFY", lambda: self.business.notify(result))
        
        return context
```

### 3. The Result (Unified Response)

The final result is a predictable, metadata-rich response, ideal for consumption by modern frontends or AI agents:

```json
{
  "data": { "id": "sub_987", "status": "submitted" },
  "message": "Submission performed successfully",
  "status": 201,
  "errors": [],
  "metadata": {
    "use_case": "UC_EDU_001",
    "rules_passed": ["R002", "R003"],
    "setters": {
      "SET_ID": "sub_987",
      "SET_LATE": false
    },
    "effects": ["EFF_DEACTIVATE", "EFF_NOTIFY"]
  }
}
```

This structure ensures that anyone (or any system) analyzing the response knows exactly which business decisions were made and which external actions were triggered, eliminating the need to "guess" system behavior.

---

### 🔗 Navigation
*   Back to the **[Manifesto](../../BRDD.md)**.
*   Get to know **[The Journey](./BRDD-STORY.md)**.
*   Access the [Main Repository](https://github.com/brdd-design/brdd).