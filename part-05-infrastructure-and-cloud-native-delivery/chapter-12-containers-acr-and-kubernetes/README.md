# Chapter 12 — Containers, Azure Container Registry, and Kubernetes

[← Part V — Infrastructure and Cloud-Native Delivery](../README.md)

This chapter follows a workload from Dockerfile to production reconciliation: immutable image, secure registry, Kubernetes desired state, managed AKS platform, GitOps delivery, and health-driven orchestration.

## Why this chapter matters

A container packages a process and filesystem, not an entire security boundary or operating model. Kubernetes automates scheduling and reconciliation, but it does not choose safe resources, identity, network policy, upgrades, probes, or application compatibility. AKS manages the control plane while customers still own substantial workload and cluster configuration.

The goal is to understand the complete chain and its failure modes—not memorize commands.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [Images, Containers, Registries, and Digests](01-images-containers-registries-and-digests.md) | Trace immutable content from build to runtime |
| 2 | [Dockerfile Layers and Multi-stage Builds](02-dockerfile-layers-and-multi-stage-builds.md) | Produce small, repeatable runtime images |
| 3 | [Container Security and Vulnerability Scanning](03-container-security-and-vulnerability-scanning.md) | Reduce build and runtime attack surface |
| 4 | [Azure Container Registry](04-azure-container-registry.md) | Operate identity, networking, replication, and lifecycle |
| 5 | [Kubernetes Core Resources](05-kubernetes-core-resources.md) | Explain desired state and workload networking |
| 6 | [Azure Kubernetes Service](06-azure-kubernetes-service.md) | Design a production AKS operating model |
| 7 | [Helm, Kustomize, and GitOps](07-helm-kustomize-and-gitops.md) | Manage reusable configuration and reconciliation |
| 8 | [Readiness, Liveness, and Startup Probes](08-readiness-liveness-and-startup-probes.md) | Prevent traffic and restart failures |

## Guided chapter lab

Build a small API into a multi-stage non-root image. Generate an SBOM, scan it, push it to ACR with unique tags, and record the digest. Configure separate CI push and AKS pull identities.

Deploy to AKS with:

- Deployment and ClusterIP Service.
- ConfigMap and secret reference.
- Resource requests/limits.
- Restricted security context.
- Pod disruption and topology considerations.
- Startup, readiness, and liveness probes.
- Network policy where supported.
- Digest-pinned image reference.
- Kustomize overlays or a versioned Helm chart.
- Flux/managed GitOps reconciliation from a protected repository.

Introduce configuration drift, an invalid readiness endpoint, a vulnerable package, and an unavailable dependency. Observe Kubernetes events, rollout status, pod logs, metrics, GitOps status, and recovery.

## Troubleshooting workflow

Start from desired state and move inward:

1. Is GitOps/controller reconciliation healthy?
2. Was the manifest rendered as intended?
3. Did admission/policy accept it?
4. Was the Pod scheduled?
5. Could the node pull the exact image?
6. Did the container start and remain alive?
7. Is it ready and selected by the Service?
8. Can ingress/gateway route to it?
9. Are DNS, network policy, identity, storage, and dependencies working?
10. Do application telemetry and business checks show success?

## Completion criteria

You are ready to complete Part V when you can trace an image digest to a running Pod, explain ACR and AKS identities, diagnose a rollout through events and probes, and prove that Git—not an imperative pipeline command—is the desired-state authority in a GitOps model.

## Chapter navigation

[← Chapter 11](../chapter-11-infrastructure-as-code/README.md) · [Part V overview](../README.md)
