<div align="center">
  <img src="../../assets/generic_cover_landscape.png" alt="BRDD Story Cover" width="800" />
</div>

# BRDD - BUSINESS RULE DRIVEN DESIGN (A Jornada)

Trabalhando em diferentes cenários e áreas de negócio no setor de TI, adquiri conhecimento comparando as mais variadas realidades. No final do dia, percebi que os problemas estruturais são quase sempre os mesmos: o desentendimento crônico entre o time de Produto/Negócios e o time de Desenvolvimento; projetos mal documentados; e sistemas legados complexos que apenas os seus criadores conseguem manter. Até mesmo o uso de IA apresenta dificuldades nesses contextos, pois demanda um gasto elevado de tokens tentando "adivinhar" a intenção por trás de um código mal estruturado, o que prejudica a performance de novos integrantes e a evolução do projeto.

Visando eliminar esse gap entre o Negócio e a TI, e organizar o código sem a necessidade de criar camadas de complexidade desnecessárias (que muitas vezes não compensam devido à elevada curva de aprendizado), desenvolvi um padrão de design baseado estritamente nas **regras de negócio**. Este design não nasceu pronto da noite para o dia. **Ele foi sendo aprimorado por diversas mentes brilhantes, em várias empresas diferentes, e testado incansavelmente nas mais diversas situações reais.** Essa evolução coletiva fez com que o padrão deixasse de ser algo "robótico" ou puramente teórico, tornando-se uma ferramenta prática, humana e validada em cenários de extrema complexidade.

### Bases do Modelo
Adotei o **MVC** e o **DDD** como fundação devido à sua simplicidade e alta aderência no mercado. O DDD permite que o modelo funcione bem tanto para grandes monolitos quanto para microsserviços. Como este modelo respeita padrões amplamente aceitos, ele pode ser adaptado aos *standards* recomendados para qualquer linguagem ou framework, exigindo apenas ajustes finos sem ferir as regras fundamentais.

---

## Papéis e Responsabilidades

Ao analisar diferentes funcionalidades em sistemas distintos, percebe-se que, de maneira abstrata, a grande maioria das *features* segue um fluxo lógico comum:

1. **Enriquecimento dos dados de entrada** (preparação do contexto);
2. **Validação das regras de negócio** (garantia da integridade);
3. **Execução da regra de negócio** (ação principal);
4. **Enriquecimento da resposta** (formatação do retorno).

Embora a ordem possa variar ou algum passo seja opcional, o fluxo invariavelmente contém: **Enriquecimento**, **Validação** e **Execução**. 

Geralmente, essas ações estão misturadas na camada de *Service*, pois não existe um consenso rígido no mercado sobre onde traçar essas fronteiras. No modelo **BRDD**, deixo explícito que essa separação deve ser feita em tipos de serviços especializados:

*   **Service:** Responsável unicamente pela execução da lógica de negócio principal.
*   **ValidationService:** Classes de responsabilidade única focadas em validar uma regra específica (ex: `CheckStockValidator`).
*   **EnrichService:** Classes focadas em buscar ou calcular dados necessários para o contexto (ex: `GetProductInfoEnricher`).
*   **ClientService:** Responsável pela comunicação com sistemas externos.
*   **Listener:** Responsável por capturar interações externas e disparar fluxos.
*   **UseCase:** O orquestrador que utiliza `ValidationServices` e `EnrichServices` para compor o fluxo de ponta a ponta.

---

## O Contexto de Negócio

Qualquer regra validada, valor definido pelo sistema ou efeito colateral deve ser identificado e atribuído explicitamente a um código que represente sua origem no contexto de negócios. Esta origem deve possuir um identificador único e estar documentada no repositório (recomenda-se o uso de arquivos `.md`, como o `BUSINESS_RULES.md`).

### Exemplo de Fluxo
Para realizar o consumo de um produto, deve-se primeiro validar se existe estoque suficiente. Após o consumo, sistemas parceiros devem ser notificados e a data de atualização deve ser registrada.

Neste cenário, mapeamos:
*   **Regra Negocial:** `(R001)` Validar disponibilidade de estoque.
*   **Use Case:** `(US001)` Consumo de produto.
*   **Efeito Colateral:** `(CE001)` Notificação de sistemas externos.
*   **Valor Carregado:** `(SET001)` Timestamp da operação.

Dessa forma, quando o time de negócios define uma nova funcionalidade, já o faz utilizando códigos que o desenvolvedor reconhecerá instantaneamente no código.

---

## Arquitetura AI-First

O grande diferencial do BRDD hoje é o suporte nativo ao desenvolvimento assistido por IA. Ao tornar o código semanticamente idêntico à documentação, criamos um ambiente onde **Agentes de IA** podem atuar como parceiros estratégicos, compreendendo o "porquê" por trás de cada linha. Isso não apenas acelera o desenvolvimento, mas garante que a arquitetura permaneça íntegra e alinhada aos objetivos de negócio à medida que o sistema escala.

---

### 🔗 Próximos Passos
Para uma formalização enxuta das regras, leia o **[Manifesto BRDD](../../BRDD.md)**.
Confira o repositório no [GitHub](https://github.com/brdd-design/brdd).