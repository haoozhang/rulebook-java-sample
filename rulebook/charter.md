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
- Gradle is acceptable where already in use.
- Azure App Service is limited to simple web applications under 5,000 lines of code with no asynchronous processing.

## Modernization Strategy (6R Guidelines)

| Application Type | Default Strategy | Override Conditions |
|------------------|------------------|---------------------|
| Java services | Replatform to Azure Kubernetes Service | None |
| Simple Java web applications | Replatform to Azure Kubernetes Service | Azure App Service may be used when the application is under 5,000 lines of code and has no asynchronous processing |

## Principles

- Use the approved Java runtime, framework, build, deployment, and container stack.
- Route all service-to-service communication through the internal ServiceMesh SDK.
- Use explicit `Result<T>` error handling for application code.
- Use InternalLogger as the sole logging framework.
- Externalize application configuration and protect sensitive values with Azure Key Vault.
- Use Azure AD for user-facing authentication and Managed Identity for service-to-service authentication.
- Apply portfolio-specific and organization-wide compliance controls.
