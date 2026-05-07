# Targets

## Target Frameworks

| Language | Target Version | Notes |
|----------|---------------|-------|
| Java | 17 (LTS) | Java 8 and Java 11 are end-of-life for internal use |
| Spring Boot | 3.x (latest stable) | All Spring Boot 2.x applications must be upgraded |
| Maven | 3.9+ | Gradle acceptable only where already in use |

## Target Compute Services

| Platform | Use When |
|----------|----------|
| Azure Kubernetes Service (AKS) | Default for all services |
| Azure App Service | Simple web apps only: under 5,000 LOC with no async processing |

## Target Data Services

<!-- No target data services specified in source document -->

## Target Integration Services

<!-- No target integration services specified in source document -->

## Target Libraries

| Source | Target | Notes |
|--------|--------|-------|
| `RestTemplate` | `com.acme.mesh.ServiceMesh` | Deprecated; no mesh integration |
| `WebClient` | `com.acme.mesh.ServiceMesh` | Bypasses mesh layer |
| `FeignClient` | `com.acme.mesh.ServiceMesh` | Bypasses mesh layer |
| `OkHttp` (direct HTTP client) | `com.acme.mesh.ServiceMesh` | Bypasses mesh layer |
| `Apache HttpClient` (direct HTTP client) | `com.acme.mesh.ServiceMesh` | Bypasses mesh layer |
| SLF4J (`@Slf4j`, `LoggerFactory.getLogger`) | `com.acme.logging.InternalLogger` | Does not integrate with internal trace context propagation |
| Log4j (any version) | `com.acme.logging.InternalLogger` | Does not integrate with internal trace context propagation |
| Logback (direct usage) | `com.acme.logging.InternalLogger` | Does not integrate with internal trace context propagation |
| `java.util.logging` | `com.acme.logging.InternalLogger` | Does not integrate with internal trace context propagation |
| `System.out.println` / `System.err.println` | `com.acme.logging.InternalLogger` | Not structured; not correlated |

## Target Artifacts

| Artifact | Location | Notes |
|----------|----------|-------|
| Container base image (build stage) | `mcr.microsoft.com/openjdk/jdk:17-ubuntu` | Used for build-time image layer |
| Container base image (runtime stage) | `mcr.microsoft.com/openjdk/jdk:17-distroless` | Used for final runtime image layer |
