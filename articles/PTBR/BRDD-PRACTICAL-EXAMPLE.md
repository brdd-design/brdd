<div align="center">
  <img src="../../assets/generic_cover_portrait.png" alt="BRDD Practical Example" width="800" />
</div>

# 🚀 BRDD na Prática: Exemplo de Implementação

Neste artigo, aplicaremos os conceitos do BRDD em um cenário real de gerenciamento de estoque para demonstrar como a separação de papéis simplifica a arquitetura e torna o código autodocumentado.

> [!TIP]
> O código completo e a estrutura de pastas deste exemplo podem ser encontrados no diretório [repositories/brdd-examples](../../repositories/brdd-examples).

---

## Cenário: Submissão de Entregas (Assignment)

Para este exemplo mais completo, vamos imaginar uma plataforma de ensino onde um aluno está submetendo uma tarefa. Precisamos garantir que ele está matriculado, que o curso está ativo e que a data limite não passou.

### 1. Mapeamento de Regras (BUSINESS_RULES.md)

Primeiro, definimos os identificadores que conectam o negócio à implementação:

*   **UseCase:** `(UC_EDU_001)` Submissão de Entregas.
*   **Regra:** `(R001)` Validar se a entrega é anterior à data limite (cut-off).
*   **Regra:** `(R002)` Validar se o aluno possui matrícula ativa no curso.
*   **Efeito:** `(EFF_DEACTIVATE)` [PRE] Desativar submissões anteriores.
*   **Efeito:** `(EFF_NOTIFY)` [POST] Notificar instrutores sobre o novo envio.
*   **Setter:** `(SET_ID)` Atribuir UUID único para a submissão.
*   **Setter:** `(SET_LATE)` Marcar como atrasado se após o `dueDate`.

### 2. Implementação do UseCase (Orquestração Completa)

O `UseCase` atua como o orquestrador do fluxo, sem precisar conhecer a implementação interna das validações ou buscas de dados. No BRDD, priorizamos serviços de responsabilidade única. Em vez de um validador genérico e inflado, criamos classes especializadas para cada regra de negócio.

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
        # 1. Inicializa o contexto para o Use Case UC_EDU_001
        context = DefaultExecutionContext(input_data)
        
        # 2. Enriquecimento: Busca dados de Matrícula, Curso e Atividade
        data = self.enrich.enrich(context, input_data)
        
        # 3. Validação: Checa R001, R002, R003...
        self.validate.validate(context, data)
        
        # Se houver falha (ex: aluno não matriculado), o fluxo para aqui
        if not context.is_valid():
            return context
            
        # 4. Efeito PRE: Limpa submissões antigas antes de salvar a nova
        context.add_effect("EFF_DEACTIVATE", lambda: self.business.clear_old(data))
        
        # 5. Execução Principal: Salva a nova entrega
        result = self.business.execute(data)
        
        # 6. Registro de Setters e Efeito POST
        context.set("SET_ID", result.id)
        context.set("SET_LATE", result.is_late)
        context.add_effect("EFF_NOTIFY", lambda: self.business.notify(result))
        
        return context
```

### 3. O Resultado (Resposta Unificada)

O resultado final é uma resposta previsível e rica em metadados, ideal para consumo por frontends modernos ou agentes de IA:

```json
{
  "data": { "id": "sub_987", "status": "submitted" },
  "message": "Entrega realizada com sucesso",
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

Essa estrutura garante que qualquer pessoa (ou sistema) que analise a resposta saiba exatamente quais decisões de negócio foram tomadas e quais ações externas foram disparadas, eliminando a necessidade de "adivinhar" o comportamento do sistema.

---

### 🔗 Navegação
*   Voltar para o **[Manifesto](../../BRDD.md)**.
*   Conhecer a **[Jornada BRDD](./BRDD-STORY.md)**.
*   Acesse o [Repositório Principal](https://github.com/brdd-design/brdd).
