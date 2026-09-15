# Charter

## Metadata

| Field | Value |
|-------|-------|
| Rulebook Name | Acme Corp Internal Technology Guidelines |
| Document Owner | Platform Engineering Team |
| Last Updated | 2025-01-15 |
| Classification | Internal |

## Scope

### Covered Applications and Languages

All Java applications in the Payments and Commerce portfolios must be modernized by Q4 2026.

### Constraints

- Java 8 and Java 11 are end-of-life for internal use.
- Spring Boot 2.x applications must be upgraded.
- Maven 3.9+ is required. Gradle is acceptable where already in use.
- Azure Kubernetes Service is the default deployment platform for all services.
- Azure App Service is permitted only for simple web applications under 5,000 lines of code with no asynchronous processing.

## Principles

- Route all service-to-service communication through `com.acme.mesh.ServiceMesh`.
- Use `com.acme.commons.Result<T>` for explicit application error handling.
- Use `com.acme.logging.InternalLogger` as the sole logging framework.
- Externalize application configuration and store sensitive values in Azure Key Vault.
- Use Azure AD with OAuth 2.0 / OIDC for user-facing authentication and Managed Identity for service-to-service authentication.
