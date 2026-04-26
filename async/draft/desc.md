# ⚡ BRDD Async

BRDD-ASYNC é uma extensão do BRDD para lidar com fluxos assíncronos.

Foi criado para resolver o problema de organizar fluxos assíncronos de maneira que seja possível identificar o fluxo por completo. Quando o fluxo é síncrono, é muito mais fácil de analisar e o trabalho da IA também é mais simples, pois você vê exatamente o que está ocorrendo. A partir do momento que o processo se torna assíncrono, o contexto passa a ser além do que você vê no código; com isso, acaba-se às vezes tendo dificuldades para entender em que passo está, qual já foi executado, qual está para ser executado e os critérios para avançar para o próximo passo.

O BRDD Async funciona da mesma maneira que o core: possui seus papéis, contextos e se baseia em códigos originários da camada de negócios que são documentados e vinculados ao código.

A diferença é que o fluxo agora se inicia pelo serviço **FlowEngineService**, que orquestra a interação entre os serviços **FlowContextLoader**, **UseCaseService** (core) e **FlowContextAccumulator**.

A partir do momento que é chamado o `UseCaseService`, o fluxo ocorre da mesma maneira que o core. Contudo, o `FlowEngineService` é quem vai selecionar o `UseCaseService` a ser executado a partir dos dados encapsulados pela interface **FlowContext** (que é construída pelo `FlowContextLoader`). Ao final da execução do `UseCaseService`, o `FlowContextAccumulator` é acionado pelo `FlowEngineService` recebendo o `FlowContext`, garantindo que na próxima interação o fluxo possa ser continuado no próximo passo.

### Papéis introduzidos nesta extensão:

*   **FlowEngineService:** Orquestra a interação de Services em cada interação assíncrona.
*   **FlowContextLoader:** Responsável por carregar o contexto do fluxo assíncrono, que nesse modelo será composto por informações de quais UseCases já foram executados, qual UseCase deve ser executado na próxima interação e critérios de avanço.
*   **FlowContextAccumulator:** Responsável por acumular o `FlowContext` e garantir que na próxima interação o fluxo possa ser continuado no próximo passo.
*   **UseCaseService (Reutilizado):** Garante a integração com o core.

### O Contexto: FlowContext
O novo contexto introduzido é o **FlowContext**, composto pelas informações necessárias para que o `FlowEngineService` possa selecionar o `UseCaseService` atual e construir a requisição necessária para o mesmo. Ele também é passado como parâmetro para o `FlowContextAccumulator` para garantir acesso a todos os metadados de execuções prévias.

### Organização da Documentação
A documentação deve ser acrescentada após a do core, no mesmo arquivo, porém em outra seção. A documentação do core deve estar desacoplada; sendo assim, o async faz referência aos códigos de usecase do core sem alterar sua organização. A estrutura fica assim:
*   **Async:** Possui a lista de fluxos presentes no projeto. Para cada fluxo: um ID, sua descrição e a lista de etapas.
*   **Etapas:** Para cada etapa deve haver o ID de etapa, pré-requisitos e o ID do usecase.

A organização da sequência de execução das etapas fica a critério do desenvolvedor, porém **não se aconselha o uso de uma lista fixa**, mas sim o uso de estratégias de etapas pré-requisitadas. Isso mantém uma lista desacoplada que permite alteração com o mínimo de manutenção e permite elaborar estratégias de fluxos mais complexas, onde uma etapa não precise esperar outras se não for um pré-requisito real.

---

## 💡 Proposta de Padronização (FlowContext)

Para garantir que a biblioteca tenha utilidade técnica (não seja apenas semântica) sem restringir o desenvolvedor, o `FlowContext` deve ser dividido em duas partes: **Core Metadata** (gerenciado pela lib) e **Payload/Bag** (gerenciado pelo dev).

### 1. Estrutura do FlowContext
*   **`current_step` (String):** ID do passo que deve ser executado agora.
*   **`history` (List<String>):** Lista de IDs de passos concluídos com sucesso. Permite que a lib valide pré-requisitos automaticamente.
*   **`status` (Enum):** `IN_PROGRESS`, `COMPLETED`, `FAILED`, `WAITING_CALLBACK`.
*   **`metadata` (Map/JSON):** Uma "sacola" de dados genéricos que persiste entre os passos.

### 2. Métodos das Interfaces
*   **FlowContextLoader:** `load(flowId, correlationId): FlowContext`
*   **FlowEngineService:** `run(FlowContext): FlowContext` (O coração da lib: valida pré-requisitos e dispara o UseCase correto).
*   **FlowContextAccumulator:** `accumulate(FlowContext): void` (Persistência).

### 3. Validação Técnica vs Semântica
Para evitar que a lib seja apenas semântica, o **FlowEngine** deve implementar a lógica de **Gating**:
*   Um UseCase só é disparado se `FlowContext.history` contiver todos os IDs listados nos `pre_requisitos` do passo atual.
*   Se o UseCase retornar sucesso, o `FlowEngine` automaticamente move o `current_step` para o próximo e adiciona o passo anterior ao `history`.

Dessa forma, a biblioteca assume a responsabilidade de **garantir a integridade do fluxo**, enquanto o desenvolvedor foca na regra de negócio de cada UseCase.