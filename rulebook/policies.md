# Policies

## Security Requirements

### Authentication & Authorization

- User-facing authentication must use Azure AD with OAuth 2.0 / OIDC.
- Service-to-service authentication must use Managed Identity.
- Legacy JAAS and LDAP authentication must be migrated.

### Secrets Management

- Application configuration must be externalized.
- Credentials, API keys, and connection strings must be stored in Azure Key Vault.
- Azure Key Vault values must be accessed through the Spring Cloud Azure Key Vault starter or Managed Identity.

### Network Security

- Service-to-service communication must use `com.acme.mesh.ServiceMesh`.
- All traffic must use TLS 1.2 or later.

### Encryption

- Data at rest must use service-managed encryption keys.
- Restricted data must use customer-managed encryption keys.

## Compliance Requirements

### Applicable Frameworks

| Framework | Key Constraints |
|-----------|-----------------|
| PCI-DSS | Required for applications in the Payments portfolio |
| SOC 2 | Required for all applications |

## Guardrails (Hard Boundaries)

### Prohibited Technologies

| Technology | Approved Alternative |
|------------|----------------------|
| Java 8 | Java 17 (LTS) |
| Java 11 | Java 17 (LTS) |
| Spring Boot 2.x | Spring Boot 3.x (latest stable) |
| `RestTemplate` | `com.acme.mesh.ServiceMesh` |
| `WebClient` | `com.acme.mesh.ServiceMesh` |
| `FeignClient` | `com.acme.mesh.ServiceMesh` |
| OkHttp for direct service calls | `com.acme.mesh.ServiceMesh` |
| Apache HttpClient for direct service calls | `com.acme.mesh.ServiceMesh` |
| SLF4J | `com.acme.logging.InternalLogger` |
| Log4j | `com.acme.logging.InternalLogger` |
| Direct Logback usage | `com.acme.logging.InternalLogger` |
| `java.util.logging` | `com.acme.logging.InternalLogger` |
| `System.out.println` / `System.err.println` | `com.acme.logging.InternalLogger` |
| Legacy JAAS authentication | Azure AD with OAuth 2.0 / OIDC |
| LDAP authentication | Azure AD with OAuth 2.0 / OIDC |

### Prohibited Patterns

| Pattern | Approved Alternative |
|---------|----------------------|
| Direct service-to-service HTTP calls | `com.acme.mesh.ServiceMesh` |
| Throwing exceptions for business logic flow control | `com.acme.commons.Result.failure(...)` |
| `try/catch` blocks used for business logic flow control | `com.acme.commons.Result<T>` |
| `@ControllerAdvice` handlers for business exceptions | Standard HTTP response wrapper translating `Result.failure()` |
| Hardcoded credentials or connection strings in `application.yml`, `application.properties`, environment variables, or source code | Azure Key Vault |

### Required Elements

- Service-to-service calls through `com.acme.mesh.ServiceMesh`.
- Application error handling through `com.acme.commons.Result<T>`.
- Logging through `com.acme.logging.InternalLogger`.
- Externalized configuration with sensitive values in Azure Key Vault.
- Azure AD with OAuth 2.0 / OIDC for user-facing authentication.
- Managed Identity for service-to-service authentication.
- TLS 1.2 or later for all traffic.
- Encryption at rest using service-managed keys, with customer-managed keys for Restricted data.

## Coding Style Guidelines

### Java

- Return `com.acme.commons.Result<T>` for business logic failures.
- Translate `Result.failure()` to HTTP status codes through the standard response wrapper.
- Use `com.acme.mesh.ServiceMesh` for all inter-service calls.
- Use `com.acme.logging.InternalLogger` for all logging.
