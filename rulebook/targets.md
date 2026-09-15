# Targets

## Target Frameworks

| Language or Tool | Target Version | Notes |
|------------------|----------------|-------|
| Java | 17 (LTS) | Java 8 and Java 11 are end-of-life for internal use |
| Spring Boot | 3.x (latest stable) | Upgrade all Spring Boot 2.x applications |
| Maven | 3.9+ | Gradle is acceptable where already in use |

## Target Compute Services

| Platform | Use When |
|----------|----------|
| Azure Kubernetes Service (AKS) | Default for all services |
| Azure App Service | Simple web applications under 5,000 lines of code with no asynchronous processing |

## Target Libraries

| Category | Source | Target | Notes |
|----------|--------|--------|-------|
| Service communication | `RestTemplate` | `com.acme.mesh.ServiceMesh` | `RestTemplate` is deprecated and has no mesh integration |
| Service communication | `WebClient` | `com.acme.mesh.ServiceMesh` | `WebClient` bypasses the mesh layer |
| Service communication | `FeignClient` | `com.acme.mesh.ServiceMesh` | `FeignClient` bypasses the mesh layer |
| Service communication | OkHttp | `com.acme.mesh.ServiceMesh` | Direct HTTP clients bypass the mesh layer |
| Service communication | Apache HttpClient | `com.acme.mesh.ServiceMesh` | Direct HTTP clients bypass the mesh layer |
| Logging | SLF4J | `com.acme.logging.InternalLogger` | Includes `@Slf4j` and `LoggerFactory.getLogger(...)` |
| Logging | Log4j | `com.acme.logging.InternalLogger` | All versions |
| Logging | Direct Logback usage | `com.acme.logging.InternalLogger` | Third-party logging frameworks do not integrate with internal trace context propagation |
| Logging | `java.util.logging` | `com.acme.logging.InternalLogger` | Third-party logging frameworks do not integrate with internal trace context propagation |
| Logging | `System.out.println` / `System.err.println` | `com.acme.logging.InternalLogger` | Use structured logging |

## Target Artifacts

| Artifact | Location | Notes |
|----------|----------|-------|
| Build container base image | `mcr.microsoft.com/openjdk/jdk:17-ubuntu` | Build stage |
| Runtime container base image | `mcr.microsoft.com/openjdk/jdk:17-distroless` | Runtime stage |
