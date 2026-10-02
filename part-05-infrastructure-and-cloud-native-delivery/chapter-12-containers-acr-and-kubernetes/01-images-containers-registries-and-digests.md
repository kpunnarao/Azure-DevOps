# Images, Containers, Registries, and Digests

[← Chapter 12](README.md) · [Next: Dockerfile Layers →](02-dockerfile-layers-and-multi-stage-builds.md)

## Mental model

- **Image:** immutable content-addressed filesystem layers plus configuration.
- **Container:** runtime instance of an image with writable state and isolation provided by the operating system/runtime.
- **Registry:** service that stores and distributes manifests and blobs.
- **Repository:** named image collection inside a registry.
- **Tag:** convenient mutable reference such as `1.4.0` or `main`.
- **Digest:** cryptographic content identifier such as `sha256:...`.

A tag resolves to a manifest; the manifest references configuration and layers. The same digest identifies the same manifest content. Deploying only `:latest` cannot prove which bytes ran because the tag may later move.

## Identity strategy

Push unique traceability tags such as release version, CI run ID, and commit SHA. After push, capture the registry digest and use it for high-assurance deployment:

```text
contoso.azurecr.io/orders@sha256:<digest>
```

Tags improve human discovery; digests provide immutable content identity. Preserve the mapping among source commit, CI run, SBOM, scan/attestation, tags, digest, and deployed environment.

A multi-platform tag may point to an OCI image index/manifest list whose digest differs from a platform-specific image manifest. Know which digest your runtime resolves and records.

## Containers are not virtual machines

Containers usually share the host kernel. Namespaces, cgroups, capabilities, seccomp/AppArmor, filesystem permissions, and runtime controls provide isolation, but privileged or vulnerable workloads can weaken it. Treat images as untrusted input until verified.

Container writable layers are ephemeral. Persist necessary state in an external service or purpose-designed volume. Logs should flow to standard output/error or a supported telemetry sink, not live only inside the container.

## Promotion

Build once and promote by digest. Import/copy the same manifest and blobs across registry boundaries when isolation requires it, verifying identity. Do not rebuild for each environment.

Before pruning a tag or manifest, understand shared layers, retention policy, active deployments, rollback references, legal evidence, and replica behavior.

## Common mistakes

- Equating tag with immutable version.
- Running a container to store durable state.
- Rebuilding an image for production.
- Signing/scanning one digest and deploying another.
- Deleting an image still referenced by a cluster.
- Assuming an SBOM proves provenance or safety.
- Confusing image vulnerability state at build time with current risk.

## Interview preparation

**Image versus container?**  
An image is immutable packaged content/configuration; a container is a running instance with runtime state.

**Tag versus digest?**  
A tag is a mutable name; a digest is a content-derived immutable identifier.

**Why can one tag have multiple platform images?**  
It can reference an OCI index that selects architecture/OS-specific manifests.

## Practical exercise

Build an image, apply two tags, push it, and inspect manifest/index and digest. Move one disposable tag and demonstrate that the tag's meaning changes while the old digest remains specific. Run the image, write to its filesystem, remove the container, and observe ephemerality.

## Official references

- [Container registry concepts](https://learn.microsoft.com/azure/container-registry/container-registry-concepts)
- [Recommendations for tagging and versioning images](https://learn.microsoft.com/azure/container-registry/container-registry-image-tag-version)

[Next: Dockerfile Layers and Multi-stage Builds →](02-dockerfile-layers-and-multi-stage-builds.md)
