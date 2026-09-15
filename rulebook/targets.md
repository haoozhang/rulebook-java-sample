# Targets

Approved target technologies for Acme Corp Java modernization.

## Target Frameworks

| Language or Tool | Target Version | Notes |
|------------------|----------------|-------|
| Java | 17 LTS | Java 8 and Java 11 are end-of-life for internal use |
| Spring Boot | 3.x latest stable | All Spring Boot 2.x applications must be upgraded |
| Maven | 3.9 or later | Approved build tool |
| Gradle | Existing application version | Acceptable only where already in use |

## Target Compute Services

| Platform | Use When |
|----------|----------|
| Azure Kubernetes Service | Default for all services |
| Azure App Service | Simple web applications under 5,000 lines of code with no asynchronous processing |

## Target Integration Services

| Service | Use When |
|---------|----------|
| `com.acme.mesh.ServiceMesh` | All service-to-service communication |
| Azure Key Vault | Storage of credentials, API keys, and connection strings |
| Azure AD with OAuth 2.0 / OIDC | All user-facing authentication |
| Managed Identity | Service-to-service authentication and approved Azure Key Vault access |

## Target Libraries

| Category | Source | Target | Notes |
|----------|--------|--------|-------|
| Service communication | `RestTemplate` | `com.acme.mesh.ServiceMesh` | All inter-service calls must use the ServiceMesh SDK |
| Service communication | `WebClient` | `com.acme.mesh.ServiceMesh` | All inter-service calls must use the ServiceMesh SDK |
| Service communication | `FeignClient` | `com.acme.mesh.ServiceMesh` | All inter-service calls must use the ServiceMesh SDK |
| Service communication | OkHttp, Apache HttpClient, or another direct HTTP client | `com.acme.mesh.ServiceMesh` | All inter-service calls must use the ServiceMesh SDK |
| Error handling | Business-logic exceptions | `com.acme.commons.Result<T>` | HTTP responses may translate failures through the standard response wrapper |
| Error handling | Flow-control `try/catch` blocks | `com.acme.commons.Result<T>` | Application code must use explicit result handling |
| Error handling | `@ControllerAdvice` for business exceptions | `com.acme.commons.Result<T>` and the standard response wrapper | Framework-level handling of system exceptions remains allowed |
| Logging | SLF4J | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework |
| Logging | Log4j | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework |
| Logging | Direct Logback usage | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework |
| Logging | `java.util.logging` | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework |
| Logging | `System.out.println` and `System.err.println` | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework |
| Authentication | JAAS | Azure AD with OAuth 2.0 / OIDC | Must be migrated during modernization |
| Authentication | LDAP | Azure AD with OAuth 2.0 / OIDC | Must be migrated during modernization |

## Target Artifacts

| Artifact | Location | Notes |
|----------|----------|-------|
| Java build container image | `mcr.microsoft.com/openjdk/jdk:17-ubuntu` | Build stage |
| Java runtime container image | `mcr.microsoft.com/openjdk/jdk:17-distroless` | Runtime stage |
