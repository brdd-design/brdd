# 📋 Checklist de Publicação BRDD

Para que as bibliotecas BRDD sejam profissionais e confiáveis, cada implementação `repositories` deve seguir este padrão de metadados e publicação.

## 👤 Informações Requeridas (Metadados)
Toda biblioteca deve incluir:
- **Nome:** `brdd-<language>` (ex: `brdd-python`)
- **Versão:** Seguindo Semantic Versioning (ex: `0.1.0`)
- **Descrição:** "Business Rule Driven Design (BRDD) core components for <Language>."
- **Autor:** Defol Tech
- **Contato:** `defoltech@gmail.com`
- **Licença:** MIT (Recomendado).
- **Repositório:** Link para o repositório específico no GitHub (Organização `brdd-design`).

---

## 🚀 Requisitos por Ecossistema

### 🐍 Python (PyPI)
- **Conta:** [pypi.org](https://pypi.org/)
- **Arquivo:** `pyproject.toml`
- **Comandos:** `python -m build`, `twine upload dist/*`

### 🐹 Go (Go Modules)
- **Conta:** Apenas conta no GitHub.
- **Arquivo:** `go.mod`
- **Ação:** Criar uma Tag Git (ex: `v0.1.0`) e fazer Push.

### 📜 TypeScript (NPM)
- **Conta:** [npmjs.com](https://www.npmjs.com/)
- **Arquivo:** `package.json`
- **Comando:** `npm publish --access public`

### 🦀 Rust (crates.io)
- **Conta:** [crates.io](https://crates.io/)
- **Arquivo:** `Cargo.toml`
- **Comando:** `cargo publish`

### ☕ Java (Maven Central)
- **Conta:** [Sonatype OSSRH](https://central.sonatype.org/)
- **Arquivo:** `pom.xml`
- **Requisito:** Assinatura GPG e validação de domínio do `groupId`.

### 🔷 .NET (NuGet)
- **Conta:** [nuget.org](https://www.nuget.org/)
- **Arquivo:** `.csproj`
- **Comando:** `dotnet pack`, `dotnet nuget push`

---

## 🤖 Instrução para IA
Sempre que for gerar o esqueleto de uma nova biblioteca, a IA deve preencher os campos de metadados acima com valores de exemplo ou buscar as informações de contato no contexto do projeto.
