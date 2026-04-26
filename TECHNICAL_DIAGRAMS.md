# 📐 BRDD Technical Diagrams (PlantUML)

This document contains the source code for the BRDD architecture diagrams. You can copy these blocks into any PlantUML viewer or use them as a reference for AI agents to understand the system structure.

---

## 1. High-Level Architecture
Describes the core layers and how they interact.

```puml
@startuml
skinparam actorStyle awesome
package "Client Layer" {
  [Browser/Mobile] as Client
}
package "Entry Layer" {
  [Controller] as Ctrl
  [Listener] as Lst
}
package "BRDD Core" {
  [UseCase] as UC
  [ValidationService] as Val
  [EnrichService] as Enr
  [ExecutionContext] as CTX
}
package "Infrastructure" {
  [ClientService] as Ext
  [Repository] as Repo
}

Client -> Ctrl
Client -> Lst
Ctrl -> UC
Lst -> UC
UC -> Val
UC -> Enr
UC -> Repo
UC -> Ext
UC -> CTX
@enduml
```

---

## 2. Core Execution Flow
Detailed sequence of a typical UseCase execution.

```puml
@startuml
participant "Controller" as C
participant "UseCase" as UC
participant "EnrichService" as E
participant "ValidationService" as V
participant "ExecutionContext" as CTX

C -> UC: execute(data)
UC -> CTX: init(US_CODE)
UC -> E: enrich(data)
E --> UC: DomainData
UC -> V: validate(DomainData)
V -> CTX: addError(R_CODE) if invalid
UC -> CTX: is_valid()
alt is valid
    UC -> UC: process logic
    UC -> CTX: addEffect(CE_CODE)
    UC -> CTX: set(SET_CODE, val)
end
UC -> C: return ExecutionReport
@enduml
```

---

## 3. Data Transformation (4 Layers)
How data flows through the 4 semantic layers of BRDD.

```puml
@startuml
node "External Domain" as Ext
node "Business Domain" as Biz
node "DTO" as DTO
node "Models" as Mod

Ext --> Biz : via Client/Listener
Biz --> DTO : via UseCase Return
Biz --> Mod : via Repository
Mod --> Biz : via Repository
@enduml
```

---

## 4. Class / Interface Structure
Core interfaces based on the standard library implementations (TypeScript/C#/etc).

```puml
@startuml
interface IValidationContext {
  + addError(code: string, message: string)
  + hasErrors(): boolean
  + getErrors(): IBaseError[]
}

interface IExecutionContext {
  + data: any
  + validation: IValidationContext
  + setters: string[]
  + effects: string[]
  + set(code: string, value: any)
  + addEffect(code: string)
  + is_valid(): boolean
}

interface IUseCase<TInput, TOutput> {
  + execute(input: TInput): IExecutionContext
}

class StandardResponse {
  + data: any
  + message: string
  + status: int
  + errors: IBaseError[]
  + setters: string[]
  + effects: string[]
}

IExecutionContext *-- IValidationContext
IUseCase ..> IExecutionContext : returns
StandardResponse ..> IExecutionContext : maps from
@enduml
```

---

> [!TIP]
> You can render these diagrams online at [PlantText](https://www.planttext.com/) or using the VS Code PlantUML extension.
