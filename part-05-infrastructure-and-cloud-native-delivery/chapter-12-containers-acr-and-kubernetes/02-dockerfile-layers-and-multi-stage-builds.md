# Dockerfile Layers and Multi-stage Builds

[← Images and Digests](01-images-containers-registries-and-digests.md) · [Chapter 12](README.md) · [Next: Container Security →](03-container-security-and-vulnerability-scanning.md)

## Layer-aware builds

Each Dockerfile instruction contributes to build history and usually a filesystem layer. BuildKit caching can reuse unchanged steps. Order stable, expensive dependency restore before frequently changing source compilation.

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY *.sln ./
COPY src/App/*.csproj src/App/
RUN dotnet restore
COPY . .
RUN dotnet publish src/App/App.csproj -c Release -o /out --no-restore

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /out .
USER 10001
ENTRYPOINT ["dotnet", "App.dll"]
```

Versions are illustrative. Pin an approved base, maintain it, and follow framework guidance.

## Multi-stage benefits

The build stage contains compilers and source; the runtime stage receives only published output. This reduces size and attack surface. It does not automatically remove secrets previously copied into a layer or build metadata. Use secret mounts/build-service features designed not to persist credentials, and inspect history.

## Build context

Use `.dockerignore` to exclude Git metadata, local secrets, test results, dependency caches, IaC state, and unnecessary source. A smaller context improves speed and reduces accidental leakage.

Copy dependency manifests before source to maximize cache reuse, but correctness comes first. Lock dependencies and ensure cache keys/steps change when the lock file changes.

## Repeatability and base images

A mutable base tag can resolve differently tomorrow. Pin a digest for exactness and use automation to propose updated approved digests when patches arrive. Rebuild and retest rather than mutating a running container.

Set deterministic build inputs where feasible. Record base digest, builder version, source commit, dependency locks, image digest, and SBOM.

## Runtime design

Use exec-form `ENTRYPOINT`/ `CMD` so signals reach the process correctly. Run one primary concern per container, handle SIGTERM, expose only required ports, use a non-root numeric user, and make the root filesystem read-only when the app permits. Do not bake environment secrets/configuration into the image.

## Common mistakes

- `COPY . .` before dependency restore.
- Leaving compiler/package manager in runtime image.
- Installing from floating repositories without cleanup or pinning policy.
- Passing secrets via `ARG` or `ENV`.
- Running as root by default.
- Using `latest` without recording the resolved base digest.
- Optimizing layer count while making caching or review worse.

## Interview preparation

**Why does instruction order matter?**  
A changed layer invalidates dependent cache layers. Stable dependency inputs early improve reuse.

**Does deleting a secret in a later layer remove it?**  
No. Earlier layer content may remain retrievable. Prevent it from entering the build context/layer.

**Why combine tag pinning with updates?**  
Digest pinning gives reproducibility; automated reviewed updates prevent permanent vulnerability freezing.

## Practical exercise

Build a single-stage and multi-stage image. Compare size, packages, user, history, SBOM, and cold pull. Change only source, then a lock file, and observe cache invalidation. Add a harmless marker as a fake secret, remove it later, and inspect why prevention matters.

## Official references

- [Dockerfile best practices](https://docs.docker.com/build/building/best-practices/)
- [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [ACR image-management best practices](https://learn.microsoft.com/azure/container-registry/container-registry-image-manage)

[Next: Container Security and Vulnerability Scanning →](03-container-security-and-vulnerability-scanning.md)
