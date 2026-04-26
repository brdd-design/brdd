# BRDD - BUSINESS RULE DRIVEN DESIGN

Trabalhando em diferentes cenários e áreas de negócio no mundo de TI, adquiri conhecimento comparando as mais variadas realidades. No final do dia, percebi que os maiores problemas são quase sempre os mesmos: desentendimentos entre o time de Produto/Negócios e o time de Desenvolvimento; projetos mal documentados; e sistemas legados mal organizados que apenas os criadores conseguem manter. Até mesmo o uso de IA apresenta dificuldades nesses contextos, demandando um gasto elevado de tokens e prejudicando a performance de novos funcionários.

Visando eliminar esse gap entre o Negócio e a TI, e organizar o código sem a necessidade de inventar regras complexas (que muitas vezes não compensam devido à elevada curva de aprendizado), desenvolvi um padrão de design de código baseado nas **regras de negócio**. Este design não nasceu pronto; ele foi aprimorado com o tempo, a cada projeto iniciado do zero, até ser posto à prova em um cenário extremamente complexo e funcionar com sucesso.

### Bases do Modelo
Adotei o **MVC** e o **DDD** como base devido à simplicidade, baixa curva de aprendizado e alta aderência do mercado. O DDD permite que o modelo funcione bem tanto para grandes monolitos quanto para microsserviços. Como este modelo segue padrões comuns ao mercado, ele pode ser adaptado de acordo com os *standards* recomendados para cada linguagem ou framework, exigindo apenas pequenas adaptações sem quebrar as regras fundamentais.

---

## Papéis

Ao analisar diferentes funcionalidades em diversos sistemas, elas podem inicialmente parecer distintas e complexas. No entanto, de maneira abstrata, a grande maioria das *features* segue esta ordem:

1. **Enriquecimento dos dados de entrada** (caso necessário);
2. **Validação das regras de negócio**;
3. **Execução da regra de negócio**;
4. **Enriquecimento da resposta**.

Embora algum passo possa não ser necessário, a ordem varie ou um passo seja repetido, o fluxo geralmente contém: **Enriquecimento**, **Validação** e **Execução**. 

Geralmente, essas ações estão misturadas dentro da camada de *Service* e, às vezes, dentro da mesma função, pois não existe um consenso de mercado sobre o que deve ser separado. No modelo **BRDD**, deixo explícito que essa separação deve ser feita em diferentes tipos de serviços, cada qual com sua especialidade:

* **Service:** Responsável unicamente pela execução da regra de negócio principal.
* **ValidationService:** Responsável por validar as regras negociais.
* **EnrichService:** Responsável por enriquecer dados.
* **ClientService:** Responsável por interagir com sistemas externos.
* **Listener:** Responsável por receber interações de sistemas externos (mudando o fluxo para *Event-Driven*).
* **UseCase:** Realiza a orquestração de outros serviços em casos de *features* mais complexas.

---

## Contexto

Qualquer regra validada, valor definido pelo sistema ou ação executada além da regra negocial principal deve ser identificada, mapeada e atribuída explicitamente a um código que represente sua origem no contexto de negócios. Esta origem deve possuir um código de identificação e estar documentada no repositório (recomenda-se o uso de arquivos `.md`, como o `BUSINESS_RULES.md`).

### Exemplo Prático
Para consumir um produto de um armazenamento, deve-se primeiro validar se existe quantidade suficiente. Após o consumo, os sistemas parceiros devem ser informados e a data de atualização carregada com o horário atual.

Neste cenário, temos:
* **Regra Negocial:** `(R001)` Validar se existe quantidade suficiente do produto.
* **Use Case:** `(US001)` Consumo de produto na armazenagem.
* **Efeito Colateral:** `(CE001)` Atualizar sistemas parceiros.
* **Valor Carregado:** `(SET001)` Data de atualização de negócio.

Dessa forma, quando o time de negócios definir uma *feature*, haverá códigos para identificar cada item. Se algo precisar ser alterado, a atividade mencionará o código exato do item a ser modificado.

### Implementação Técnica
Para garantir que essa origem esteja presente no código, recomenda-se o uso das interfaces `ValidationContext` e `ExecutionContext`. Sua função é semelhante às bibliotecas de *assert* para testes:
* **ValidationContext:** Utilizado para centralizar validações em um relatório uniforme. Ex: `ValidationContext.assert(businessRuleName, booleanProducer)`. Deve ser manipulado e retornado sempre por um `ValidationService`.
* **ExecutionContext:** Utilizado para iniciar o contexto da funcionalidade, adicionar efeitos colaterais ou definir valores. Ex: `ExecutionContext.init(useCaseName)`, `ExecutionContext.addEffect(effectName, type)` ou `ExecutionContext.set(setName, target, value)`. Deve ser manipulado e retornado sempre por um `UseCase`.

Ao final da execução, essas interfaces servem como um relatório de tudo o que foi processado, facilitando o entendimento humano e a eficiência de prompts de IA, que conseguem vincular trechos de código à documentação de negócios.

---

## Dados

O modelo infere que os dados sejam separados em 4 camadas para manter o desacoplamento, garantindo que fornecedores, APIs externas e consumidores não interfiram nos serviços internos.

1.  **Business Domain:** Dados trafegados entre os serviços, representando o domínio comum do negócio.
2.  **External Domain:** Utilizado por sistemas externos; deve ser convertido para o *Business Domain* antes de chegar aos *Use Cases*. É reconhecido apenas por *Clients* e *Listeners*.
3.  **DTO (Data Transfer Object):** Dados consumidos por clientes do sistema.
4.  **Models:** Dados persistidos no banco de dados. A forma de persistência não deve interferir na organização do negócio. Devem ser reconhecidos apenas pelos repositórios e convertidos antes de chegar à camada de *Service*.

---

## Pastas e Arquivos

A organização das pastas fica a critério do desenvolvedor (ex: todos os serviços em uma pasta única ou pastas separadas por tipo). O importante é que a separação por **Domínio** seja respeitada, para que dados de "Usuário", por exemplo, não se misturem aos de "Produtos". Isso garante a organização e flexibilidade estrutural do projeto.

---

## Conclusão

A definição deste design é simples e dinâmica, permitindo adaptações. A ideia central não é gerar mais uma curva de aprendizado para o time técnico, mas sim permitir uma organização superior fundamentada no que realmente importa: o Negócio.