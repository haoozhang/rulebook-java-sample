# Policies

Enforceable standards and hard boundaries for Acme Corp Java modernization.

## Security Requirements

### Authentication & Authorization

- All user-facing authentication must use Azure AD with OAuth 2.0 / OIDC.
- Service-to-service authentication must use Managed Identity.
- Legacy JAAS and LDAP authentication must be migrated during modernization.

### Secrets Management

- Application configuration must be externalized.
- Credentials, API keys, and connection strings must be stored in Azure Key Vault.
- Azure Key Vault must be accessed through the Spring Cloud Azure Key Vault starter or Managed Identity.

### Network Security

- All traffic must use TLS 1.2 or later.
- All service-to-service communication must use `com.acme.mesh.ServiceMesh`.

### Encryption

- Data at rest must be encrypted with service-managed keys.
- Restricted data must use customer-managed keys.

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
| Java 8 | Java 17 LTS |
| Java 11 | Java 17 LTS |
| Spring Boot 2.x | Spring Boot 3.x latest stable |
| `RestTemplate` | `com.acme.mesh.ServiceMesh` |
| `WebClient` | `com.acme.mesh.ServiceMesh` |
| `FeignClient` | `com.acme.mesh.ServiceMesh` |
| OkHttp, Apache HttpClient, and other direct HTTP client libraries for service-to-service communication | `com.acme.mesh.ServiceMesh` |
| SLF4J, including `@Slf4j` and `LoggerFactory.getLogger(...)` | `com.acme.logging.InternalLogger` |
| Log4j | `com.acme.logging.InternalLogger` |
| Direct Logback usage | `com.acme.logging.InternalLogger` |
| `java.util.logging` | `com.acme.logging.InternalLogger` |
| `System.out.println` and `System.err.println` | `com.acme.logging.InternalLogger` |
| JAAS | Azure AD with OAuth 2.0 / OIDC |
| LDAP | Azure AD with OAuth 2.0 / OIDC |

### Prohibited Patterns

| Pattern | Approved Alternative |
|---------|----------------------|
| Direct service-to-service calls outside the ServiceMesh SDK | `com.acme.mesh.ServiceMesh` |
| Throwing exceptions for business-logic flow control | `com.acme.commons.Result<T>` |
| Using `try/catch` blocks for flow control | `com.acme.commons.Result<T>` |
| Using `@ControllerAdvice` global handlers for business exceptions | `com.acme.commons.Result<T>` with the standard HTTP response wrapper |
| Hardcoding credentials or connection strings in `application.yml`, `application.properties`, environment variables, or source code | Azure Key Vault via the Spring Cloud Azure Key Vault starter or Managed Identity |

### Required Elements

Every modernized application must include:

- Java 17 LTS.
- Spring Boot 3.x latest stable when using Spring Boot.
- Maven 3.9 or later, or Gradle where already in use.
- Azure Kubernetes Service deployment unless the Azure App Service exception applies.
- The approved build and runtime container images.
- `com.acme.mesh.ServiceMesh` for all service-to-service communication.
- `com.acme.commons.Result<T>` for application error handling.
- `com.acme.logging.InternalLogger` as the sole logging framework.
- Externalized application configuration.
- Azure Key Vault for credentials, API keys, and connection strings.
- Azure AD with OAuth 2.0 / OIDC for user-facing authentication.
- Managed Identity for service-to-service authentication.
- TLS 1.2 or later for all traffic.
- Encryption at rest with customer-managed keys for Restricted data and service-managed keys otherwise.
- SOC 2 controls.
- PCI-DSS controls when the application belongs to the Payments portfolio.

## Validation & Quality Gates

### Modernization Completion Gates

| Gate | Passing Condition |
|------|-------------------|
| Schedule | Payments and Commerce Java applications are modernized by Q4 2026 |
| Runtime | Application runs on Java 17 LTS |
| Framework | Spring Boot applications use Spring Boot 3.x latest stable |
| Build | Application uses Maven 3.9 or later, or retains Gradle where already in use |
| Deployment | Application uses Azure Kubernetes Service, or qualifies for the Azure App Service exception |
| Containers | Build and runtime stages use the approved container images |
| Service communication | All service-to-service calls use `com.acme.mesh.ServiceMesh` |
| Error handling | Application code uses `com.acme.commons.Result<T>` and does not use exceptions for business-logic flow control |
| Logging | Application uses only `com.acme.logging.InternalLogger` |
| Configuration | Configuration is externalized and sensitive values are stored in Azure Key Vault |
| Authentication | User-facing authentication uses Azure AD with OAuth 2.0 / OIDC and service authentication uses Managed Identity |
| Transport security | All traffic uses TLS 1.2 or later |
| Data encryption | Data at rest is encrypted and Restricted data uses customer-managed keys |
| Compliance | All applications meet SOC 2 controls and Payments applications also meet PCI-DSS requirements |
