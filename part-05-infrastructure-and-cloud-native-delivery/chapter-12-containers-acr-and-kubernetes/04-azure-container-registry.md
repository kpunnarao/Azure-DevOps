# Azure Container Registry

[← Container Security](03-container-security-and-vulnerability-scanning.md) · [Chapter 12](README.md) · [Next: Kubernetes Core Resources →](05-kubernetes-core-resources.md)

## Registry operating model

Azure Container Registry (ACR) stores OCI/Docker images and other OCI artifacts. Production design must cover tier, region, network, identity, repository permissions, image lifecycle, scanning, replication, and disaster recovery.

Create the registry in a dedicated or deliberately protected resource group. Deleting an application sandbox must not accidentally delete its shared image supply chain.

## Authentication and authorization

Use individual Microsoft Entra authentication for developers and service identities for automation. Separate:

- CI identity: push to approved repositories.
- AKS/kubelet identity: pull only.
- Promotion/import identity: narrowly scoped content movement.
- Administrators: registry configuration, not routine builds.

Prefer fine-grained repository permissions/appropriate ACR roles where supported. Disable or avoid the registry admin account for routine production use. Grant Azure DevOps access through a protected service connection with short-lived/federated credentials where supported.

## Network and performance

Keep registry close to compute to reduce pull latency and egress. Premium supports capabilities such as geo-replication and private networking. Geo-replication improves regional availability/performance, but understand replication lag and operational behavior before using it for failover.

Private endpoints restrict data-plane access but require DNS, agent connectivity, and management-path design. A hosted build agent may not reach a private-only registry without an approved network path.

## Lifecycle

Use unique tags and deploy digests. Configure retention/cleanup carefully for untagged manifests, old tags, caches, Helm/OCI artifacts, active deployments, and rollback. A running node may have cached an image that a new node cannot pull after deletion.

Quarantine or block compromised digests through policy and identify deployed consumers. Rebuild from corrected dependencies rather than patching an existing image.

ACR Tasks can build, run, or react to base-image changes; decide whether your governed pipeline or registry task is the authoritative producer and preserve equivalent provenance.

## Multiregion and import

Use ACR import or controlled OCI copy when moving exact content without local pull/push, and verify the resulting digest/manifest semantics. For global workloads, decide whether one geo-replicated registry or separate registries better matches isolation and recovery requirements.

## Interview preparation

**Why separate push and pull identities?**  
A compromised runtime should not publish replacement images, and a build pipeline does not need cluster authority.

**Why deploy by digest?**  
A digest locks workload identity to content even if tags move.

**Does private endpoint make ACR secure?**  
It reduces network exposure but does not replace identity, RBAC, scanning, retention, or client security.

## Practical exercise

Create a sandbox ACR, disable routine admin use, give CI push and workload pull access, push a uniquely tagged image, and deploy its digest. Test unauthorized push, tag movement, cleanup, and a private-network connectivity diagnosis.

## Official references

- [ACR best practices](https://learn.microsoft.com/azure/container-registry/container-registry-best-practices)
- [Authenticate with ACR](https://learn.microsoft.com/azure/container-registry/container-registry-authentication)
- [ACR roles and permissions](https://learn.microsoft.com/azure/container-registry/container-registry-rbac-built-in-roles-overview)

[Next: Kubernetes Core Resources →](05-kubernetes-core-resources.md)
