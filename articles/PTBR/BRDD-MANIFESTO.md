<div align="center">
  <img src="../../assets/generic_cover_landscape.png" alt="BRDD Manifesto Cover" width="800" />
</div>

# 📜 Manifesto BRDD: As 4 Regras de Ouro

O **Business Rule Driven Design (BRDD)** não é apenas uma estrutura de pastas; é um compromisso técnico fundamentado em quatro pilares inegociáveis. O objetivo é garantir que o Código e o Negócio operem sob a mesma gramática.

## 🏛 Os 4 Pilares

### 1. Unique Rule Coding (Identificação Única)
Toda regra de negócio, validação ou efeito colateral deve possuir um código identificador único (ex: `R001`). No BRDD, o código não é apenas lógica; ele é o elo direto com a documentação de negócio. Se uma regra muda no papel, o desenvolvedor sabe exatamente qual linha de código deve ser ajustada.

### 2. Execution Context Narrative (Narrativa de Execução)
Use Cases não devem retornar apenas dados brutos; eles devem narrar o que aconteceu durante o processo através do `ExecutionContext`. O retorno do sistema torna-se um relatório de execução: o que foi validado, o que foi alterado e quais efeitos externos foram disparados. Isso simplifica drasticamente o debug e a auditoria de processos.

### 3. Service Specialization (Especialização de Papéis)
Para manter a manutenibilidade, a lógica deve ser dividida em papéis semânticos claros, evitando serviços com sobrecarga de responsabilidades. No BRDD, cada serviço tem um propósito definido:
*   **UseCase:** Orquestrador do fluxo lógico.
*   **ValidateService:** Especialista em validar a integridade de uma regra.
*   **EnrichService:** Responsável por preparar o contexto de dados.
*   **Client/Listener:** Responsável pela integração com domínios externos.

### 4. Unified Response Pattern (Padronização de Resposta)
Toda interação via API deve seguir um formato JSON rigoroso e previsível. Isso garante que qualquer consumidor — seja uma interface de usuário ou um agente de IA — saiba exatamente como interpretar o sucesso ou a falha de uma operação.

---

## 🤖 Design AI-First por Natureza
O BRDD foi desenhado especificamente para maximizar a eficiência de IAs e Copilots. Ao isolar regras em metadados e serviços especializados, eliminamos a ambiguidade técnica.

- **Master Prompts:** A documentação em `.md` atua como uma especificação técnica "viva" e executável.
- **Segurança Arquitetural:** Com fronteiras bem definidas, impedimos que a IA introduza efeitos colaterais indesejados durante a implementação de novas validações.

---

### 🔗 Próximos Passos
Veja um exemplo real no **[Exemplo Prático](./BRDD-PRACTICAL-EXAMPLE.md)**.
Entenda a origem deste padrão na **[Jornada BRDD](./BRDD-STORY.md)**.
