# Charter

## Metadata

| Field | Value |
|-------|-------|
| Playbook Name | Acme Corp Modernization Playbook |
| Version | 1.0.0 |
| Changelog | Initial version derived from internal-tech-guidelines.md (last updated 2025-01-15) |

## Scope

### Covered Applications and Languages

All Java applications in the **Payments** and **Commerce** portfolios. Modernization target deadline: **Q4 2026**.

### Application Types

**Included:**

- Java-based backend services and APIs in the Payments and Commerce portfolios

**Excluded:**

- Applications outside the Payments and Commerce portfolios (not covered by these guidelines)

### Custom Libraries

| Library | Artifact ID | Purpose |
|---------|-------------|---------|
| ServiceMesh SDK | `com.acme.mesh.ServiceMesh` | Mandatory service-to-service communication (circuit breaking, mTLS, tracing, canary routing) |
| Result wrapper | `com.acme.commons.Result` | Explicit error handling — wraps success or failure values |
| InternalLogger | `com.acme.logging.InternalLogger` | Sole approved logging framework with trace correlation and structured JSON output |

### Constraints

- Build tool must be Maven 3.9+; Gradle is acceptable only where already in use.
- Java 8 and Java 11 are end-of-life for internal use.
- Spring Boot 2.x applications must be upgraded to Spring Boot 3.x.

## Modernization Strategy (6R Guidelines)

| Application Type | Default Strategy | Override Conditions |
|------------------|-----------------|---------------------|
| Java service / API (general) | Replatform → AKS | None — AKS is the default for all services |
| Simple Java web app | Replatform → Azure App Service | Applies only when: application is under 5,000 LOC **and** has no async processing |

## Principles

- All service-to-service communication must go through the internal ServiceMesh SDK — direct HTTP calls bypassing the mesh are prohibited.
- All application code must use the `Result<T>` pattern for error handling; throwing exceptions for business logic flow control is prohibited.
- All applications must use `InternalLogger` as the sole logging framework to ensure log correlation across the observability stack.
- Sensitive configuration values (credentials, API keys, connection strings) must be externalized to Azure Key Vault; hardcoded secrets are prohibited.
