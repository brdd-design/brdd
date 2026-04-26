# 🤖 BRDD Automation Guide (CI/CD Strategy)

This document outlines the recommended CI/CD strategy for the BRDD ecosystem to ensure automated and standardized package publication.

## 🎯 Goals
1.  **Standardization:** Maintain a similar release flow across all 7+ languages.
2.  **Trust:** Automated tests must pass before any publication.
3.  **Traceability:** Every release should be linked to a Git Tag (Semantic Versioning).

---

## 🛠 Publication Workflow (Recommended)

For each repository, we recommend using **GitHub Actions** with the following flow:

1.  **Trigger:** Pushing a new tag (e.g., `v0.1.1`) or a PR to `main`.
2.  **Validation:** Run unit tests and linting.
3.  **Build:** Generate the distribution package (Wheel, Jar, Gem, etc.).
4.  **Publish:** Upload to the respective registry using Secret Tokens.

### Language-Specific Recommendations

| Language | Registry | Tool | GitHub Action |
| :--- | :--- | :--- | :--- |
| **Python** | PyPI | `twine` / `pdm` | `pypa/gh-action-pypi-publish` |
| **TypeScript** | NPM | `npm publish` | `actions/setup-node` (registry-url) |
| **.NET** | NuGet | `dotnet nuget push` | `actions/setup-dotnet` |
| **Rust** | Crates.io | `cargo publish` | `katyo/publish-crates` |
| **Java** | Maven Central | `mvn deploy` | `actions/setup-java` (server-id: ossrh) |
| **Go** | Go Proxy | `git tag` | Automatic (via GitHub Tags) |
| **PHP** | Packagist | `git tag` | Automatic (via Webhook) |

---

## 📝 Release Instructions (For Maintainers)

To trigger an automated release:

1.  **Update Version:** Bump the version in the project metadata (`pyproject.toml`, `package.json`, `pom.xml`, etc.).
2.  **Commit:** `git commit -am "chore: bump version to v0.1.x"`
3.  **Tag:** `git tag v0.1.x`
4.  **Push:** `git push origin main --tags`

> [!TIP]
> Each repository's README should link to this guide and specify its particular automation status.

---

## 🔒 Security Best Practices
- Use **GitHub Environments** to protect publication secrets.
- Use **Trusted Publishers** (OpenID Connect) where supported (PyPI, Google Cloud, etc.) to eliminate the need for long-lived tokens.
