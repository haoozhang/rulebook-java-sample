# Policies

## Naming & Metadata Standards

### Resource Naming Patterns

<!-- No resource naming patterns specified in source document -->

### Tagging Requirements

<!-- No tagging requirements specified in source document -->

## Security Requirements

### Authentication & Authorization

- All user-facing authentication must use **Azure AD with OAuth 2.0 / OIDC**.
- Service-to-service authentication must use **Managed Identity**.
- Legacy JAAS and LDAP authentication must be migrated as part of modernization.

### Secrets Management

- Sensitive values (credentials, API keys, connection strings) must be stored in **Azure Key Vault** and accessed via the Spring Cloud Azure Key Vault starter or Managed Identity.
- Hardcoded credentials or connection strings in `application.yml`, `application.properties`, environment variables, or source code are prohibited.

### Network Security

- All service-to-service communication must go through the internal ServiceMesh SDK (`com.acme.mesh.ServiceMesh`), which provides automatic mTLS termination, circuit breaking, distributed tracing, and canary routing.
- All traffic must use **TLS 1.2+**.

### Encryption

- Data at rest must be encrypted using service-managed keys.
- Customer-managed keys are required for **Restricted** data classification.

## Compliance Requirements

### Applicable Frameworks

| Framework | Key Constraints |
|-----------|----------------|
| PCI-DSS | Applies to all applications in the **Payments** portfolio |
| SOC 2 | Applies to **all** applications |

### Data Classification

- Customer-managed encryption keys are required for data classified as **Restricted**.

## Guardrails (Hard Boundaries)

### Prohibited Technologies

| Technology | Reason | Approved Alternative |
|-----------|--------|---------------------|
| `RestTemplate` | Deprecated; no mesh integration | `com.acme.mesh.ServiceMesh` |
| `WebClient` | Bypasses mesh layer | `com.acme.mesh.ServiceMesh` |
| `FeignClient` | Bypasses mesh layer | `com.acme.mesh.ServiceMesh` |
| `OkHttp` (direct) | Bypasses mesh layer | `com.acme.mesh.ServiceMesh` |
| Apache HttpClient (direct) | Bypasses mesh layer | `com.acme.mesh.ServiceMesh` |
| SLF4J (`@Slf4j`, `LoggerFactory.getLogger`) | Does not integrate with internal trace context propagation | `com.acme.logging.InternalLogger` |
| Log4j (any version) | Does not integrate with internal trace context propagation | `com.acme.logging.InternalLogger` |
| Logback (direct usage) | Does not integrate with internal trace context propagation | `com.acme.logging.InternalLogger` |
| `java.util.logging` | Does not integrate with internal trace context propagation | `com.acme.logging.InternalLogger` |
| `System.out.println` / `System.err.println` | Unstructured; not correlated with trace context | `com.acme.logging.InternalLogger` |
| Java 8 | End-of-life for internal use | Java 17 (LTS) |
| Java 11 | End-of-life for internal use | Java 17 (LTS) |
| Spring Boot 2.x | Must be upgraded | Spring Boot 3.x (latest stable) |
| Legacy JAAS authentication | Must be migrated | Azure AD with OAuth 2.0 / OIDC |
| LDAP authentication | Must be migrated | Azure AD with OAuth 2.0 / OIDC |
| Azure Key Vault | Must be migrated | Azure App Config |

### Prohibited Patterns

| Pattern | Reason | Approved Alternative |
|---------|--------|---------------------|
| `throw new XxxException(...)` for business logic errors | Causes unhandled exception propagation; mandated by P0-2024-0847 post-incident review | `Result.failure(ErrorCode, message)` using `com.acme.commons.Result` |
| `try/catch` blocks used for flow control | Mandated by P0-2024-0847 post-incident review | `Result<T>` pattern |
| `@ControllerAdvice` for business exceptions | Business exceptions must not be handled globally; system exceptions (e.g., `OutOfMemoryError`) remain acceptable at framework level | `Result<T>` pattern with HTTP response translation layer |
| Direct service-to-service HTTP calls without ServiceMesh SDK | Bypasses circuit breaking, mTLS, distributed tracing, and canary routing | `com.acme.mesh.ServiceMesh` |

### Required Elements

Every modernized application must include:

#### Cloud Resources

- Secrets and sensitive configuration stored in **Azure Key Vault**, accessed via Spring Cloud Azure Key Vault starter or Managed Identity.

#### Monitoring

- All logging must use `com.acme.logging.InternalLogger` to ensure distributed trace ID, team ownership tag, environment/region tags, and structured JSON output are injected into every log entry.

#### CI/CD

<!-- No CI/CD requirements specified in source document -->

#### Testing

<!-- No testing requirements specified in source document -->

### Approved Regions / Residency Constraints

<!-- No region or residency constraints specified in source document -->

## Validation & Quality Gates

### Required Scanners/Tools

<!-- No required scanners or tools specified in source document -->

### Pipeline Gates

<!-- No pipeline gates specified in source document -->

### Confidence Thresholds

<!-- No confidence thresholds specified in source document -->

## Coding Style Guidelines

### Coding Standards

- All error-returning methods must return `Result<T>` (`com.acme.commons.Result`) instead of throwing exceptions.
- All inter-service calls must use `com.acme.mesh.ServiceMesh` — no direct instantiation of HTTP clients.
- All log statements must use `com.acme.logging.InternalLogger` static methods (`info`, `warn`, `error`).

### Frontend Style Guidelines

<!-- No frontend style guidelines specified in source document -->
