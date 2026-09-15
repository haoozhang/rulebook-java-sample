# Targets

Approved target technologies for Acme Corp Java modernization.

## Target Frameworks

| Language / Tool | Target Requirement | Notes |
|-----------------|--------------------|-------|
| Java | 17 (LTS) | Required for all in-scope applications. |
| Spring Boot | 3.x, latest stable | Upgrade all Spring Boot 2.x applications. |
| Maven | 3.9+ | Approved build tool. |
| Gradle | Acceptable where already in use | No target version specified. |

## Target Compute Services

| Platform | Use When |
|----------|----------|
| Azure Kubernetes Service (AKS) | Default for all services. |
| Azure App Service | Simple web applications under 5,000 lines of code with no asynchronous processing. |

## Target Integration Services

| Service | Use When |
|---------|----------|
| Azure Key Vault | Store credentials, API keys, and connection strings. |
| Azure AD with OAuth 2.0 / OIDC | User-facing authentication. |
| Managed Identity | Service-to-service authentication and approved Azure Key Vault access. |
| ServiceMesh SDK (`com.acme.mesh.ServiceMesh`) | All service-to-service communication. |

## Target Libraries

| Category | Source | Target | Notes |
|----------|--------|--------|-------|
| Service communication | `RestTemplate` | `com.acme.mesh.ServiceMesh` | All service-to-service calls must use the ServiceMesh SDK. |
| Service communication | `WebClient` | `com.acme.mesh.ServiceMesh` | All service-to-service calls must use the ServiceMesh SDK. |
| Service communication | `FeignClient` | `com.acme.mesh.ServiceMesh` | All service-to-service calls must use the ServiceMesh SDK. |
| Service communication | OkHttp, Apache HttpClient, or another direct HTTP client | `com.acme.mesh.ServiceMesh` | All service-to-service calls must use the ServiceMesh SDK. |
| Error handling | Business-logic exceptions | `com.acme.commons.Result<T>` | Use explicit success and failure results. |
| Error handling | `try/catch` flow control | `com.acme.commons.Result<T>` | Use explicit success and failure results. |
| Error handling | `@ControllerAdvice` for business exceptions | Standard response wrapper for `Result.failure()` | Translate failures to HTTP status codes without business exception handlers. |
| Logging | SLF4J | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework. |
| Logging | Log4j | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework. |
| Logging | Direct Logback usage | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework. |
| Logging | `java.util.logging` | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework. |
| Logging | `System.out` and `System.err` | `com.acme.logging.InternalLogger` | InternalLogger is the sole logging framework. |
| Authentication | JAAS | Azure AD with OAuth 2.0 / OIDC | Migrate during modernization. |
| Authentication | LDAP | Azure AD with OAuth 2.0 / OIDC | Migrate during modernization. |

## Target Artifacts

| Artifact | Location | Notes |
|----------|----------|-------|
| Build container base image | `mcr.microsoft.com/openjdk/jdk:17-ubuntu` | Required build image. |
| Runtime container base image | `mcr.microsoft.com/openjdk/jdk:17-distroless` | Required runtime image. |
