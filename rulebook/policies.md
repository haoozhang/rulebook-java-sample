# Policies

Enforceable standards and hard boundaries for Acme Corp Java modernization.

## Security Requirements

### Authentication & Authorization

- All user-facing authentication must use Azure AD with OAuth 2.0 / OIDC.
- Service-to-service authentication must use Managed Identity.
- Legacy JAAS and LDAP authentication must be migrated during modernization.

### Secrets Management

- Application configuration should be externalized.
- Credentials, API keys, and connection strings must be stored in Azure Key Vault and accessed through the Spring Cloud Azure Key Vault starter or Managed Identity.
- Credentials and connection strings must not be hardcoded in `application.yml`, `application.properties`, environment variables, or source code.

### Network Security

- All traffic must use TLS 1.2 or later.

### Encryption

- Data at rest must be encrypted with service-managed keys.
- Restricted data must be encrypted with customer-managed keys.

## Compliance Requirements

### Applicable Frameworks

| Framework | Key Constraints |
|-----------|-----------------|
| PCI-DSS | Applies to applications in the Payments portfolio. |
| SOC 2 | Applies to all applications. |

## Guardrails (Hard Boundaries)

### Prohibited Technologies

| Technology | Approved Alternative |
|------------|----------------------|
| Java 8 | Java 17 (LTS) |
| Java 11 | Java 17 (LTS) |
| Spring Boot 2.x | Spring Boot 3.x, latest stable |
| `RestTemplate` | `com.acme.mesh.ServiceMesh` |
| `WebClient` | `com.acme.mesh.ServiceMesh` |
| `FeignClient` | `com.acme.mesh.ServiceMesh` |
| Direct HTTP client libraries, including OkHttp and Apache HttpClient | `com.acme.mesh.ServiceMesh` |
| SLF4J | `com.acme.logging.InternalLogger` |
| Log4j | `com.acme.logging.InternalLogger` |
| Direct Logback usage | `com.acme.logging.InternalLogger` |
| `java.util.logging` | `com.acme.logging.InternalLogger` |
| JAAS | Azure AD with OAuth 2.0 / OIDC |
| LDAP | Azure AD with OAuth 2.0 / OIDC |

### Prohibited Patterns

| Pattern | Approved Alternative |
|---------|----------------------|
| Service-to-service calls that bypass the ServiceMesh SDK | `com.acme.mesh.ServiceMesh` |
| Throwing exceptions for business-logic flow control | `com.acme.commons.Result<T>` |
| `try/catch` blocks used for flow control | `com.acme.commons.Result<T>` |
| `@ControllerAdvice` global handlers for business exceptions | Translate `Result.failure()` through the standard response wrapper. |
| Logging through `System.out.println` or `System.err.println` | `com.acme.logging.InternalLogger` |
| Hardcoded credentials or connection strings in configuration files, environment variables, or source code | Azure Key Vault through the Spring Cloud Azure Key Vault starter or Managed Identity |

### Required Elements

Every modernized application must include:

- `com.acme.mesh.ServiceMesh` for all service-to-service communication.
- `com.acme.commons.Result<T>` for application error handling.
- `com.acme.logging.InternalLogger` as the sole logging framework.
- Azure Key Vault for credentials, API keys, and connection strings.
- Azure AD with OAuth 2.0 / OIDC for user-facing authentication.
- Managed Identity for service-to-service authentication.
- TLS 1.2 or later for all traffic.
- Encryption at rest using service-managed keys, or customer-managed keys for Restricted data.
- SOC 2 control compliance.
- PCI-DSS compliance for Payments portfolio applications.
