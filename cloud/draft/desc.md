# ☁️ BRDD Cloud

BRDD-CLOUD é uma extensão do BRDD para lidar com a organização e rastreabilidade de recursos em nuvem.

Muitas vezes, os recursos em nuvem tornam-se difíceis de gerenciar à medida que crescem. Não existe um guia de organização comum a todos os provedores (AWS, Azure, GCP); as preocupações costumam focar apenas em segurança, custo e otimização. No entanto, uma organização que permita ter uma ideia clara dos recursos e como eles estão relacionados facilita a manutenção e, crucialmente, melhora a interação com Agentes de IA.

Por exemplo, não existe uma convenção universal para nomes, tags e localização dos recursos que esteja alinhada ao domínio de negócio.

## 🚀 Princípios do BRDD-CLOUD

*   **Fonte da Verdade (Single Source of Truth):** Deve existir um repositório central que sirva como o catálogo mestre de todos os recursos em nuvem. Este catálogo deve ser mantido atualizado (manualmente ou via scripts) para que a IA possa entender a infraestrutura e relacioná-la aos Use Cases do código.
*   **Links Diretos:** O catálogo deve possuir o link direto para o recurso no console da nuvem (Cloud Console), permitindo acesso instantâneo.
*   **Central de Use Cases:** Como os códigos de Use Cases e fluxos estão espalhados por vários repositórios, o repositório BRDD-CLOUD deverá funcionar como uma "Central de Use Cases". Cada vez que um Use Case for implementado, ele deve referenciar este catálogo, facilitando a pesquisa global de onde e como cada funcionalidade é utilizada.
*   **Abstração Neutra (Neutral Naming):** Para serviços com finalidades parecidas mas nomes diferentes entre provedores (ex: S3 vs Blob Storage), o BRDD-CLOUD utiliza nomes neutros mapeados. Isso evidencia as diferenças técnicas mas mantém o contexto de negócio unificado.
*   **Diagrama de Recursos Cloud:** O repositório deve conter o diagrama visual (definido no padrão BRDD-CLOUD) para visualização intuitiva da localização e interação dos recursos.

---
Guia de organização de recursos em nuvem seguindo o padrão BRDD. Em desenvolvimento.
