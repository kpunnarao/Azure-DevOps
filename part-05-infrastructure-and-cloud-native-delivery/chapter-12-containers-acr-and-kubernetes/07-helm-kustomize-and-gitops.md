# Helm, Kustomize, and GitOps

[← Azure Kubernetes Service](06-azure-kubernetes-service.md) · [Chapter 12](README.md) · [Next: Kubernetes Probes →](08-readiness-liveness-and-startup-probes.md)

## Different responsibilities

- **Helm** packages and templates Kubernetes resources into versioned charts with values and release history.
- **Kustomize** composes plain YAML bases with overlays and patches without a template language.
- **GitOps** makes version-controlled desired state authoritative and uses in-cluster controllers such as Flux or Argo CD to reconcile it.

These are complementary. Flux can reconcile Kustomizations and Helm releases.

## Selection

Choose Helm when distributing a reusable application with structured configuration, dependencies, chart versioning, and release semantics. Keep values shallow, document them, lock dependencies, render/lint/test output, and review CRD lifecycle separately.

Choose Kustomize when teams want recognizable base YAML plus bounded environment differences. Avoid overlays made of long fragile patches that depend on line/shape assumptions.

Choose GitOps when continuous pull-based reconciliation, fleet consistency, drift remediation, and auditable desired state are needed. The delivery pipeline updates an image digest or configuration in a protected Git repository; the cluster controller pulls and reconciles. The pipeline does not also run `kubectl apply` against the same objects.

## GitOps trust model

Protect the desired-state repository, controller service account, source credentials, chart/OCI sources, commit/tag references, and promotion automation. Separate namespaces/tenants and restrict cross-namespace references. Pin dependencies and images. Sign/verify commits or artifacts where policy requires.

Secrets should not be plaintext in Git. Use an approved encrypted-secret workflow or external secret provider, and protect decryption identity.

## Reconciliation and drift

Continuous reconciliation can correct unauthorized manual change, but may also undo an emergency fix. Establish a pause/suspend and reconciliation procedure with expiry and audit. After an incident, reconcile the source of truth before resuming.

Monitor source fetch, artifact/render, apply, health, dependency ordering, and controller version. A Git merge is not a successful deployment until reconciliation and workload health succeed.

## Rollback

Git revert produces a new auditable desired state. Helm rollback can change in-cluster release revision, but in GitOps the repository must still reflect the desired version or the controller may undo the manual rollback. For stateful changes, manifest reversal alone may be unsafe.

## Interview preparation

**Helm versus Kustomize?**  
Helm is package/template/value oriented; Kustomize overlays and patches plain resources. Choose based on reuse and configuration complexity.

**Push CD versus pull GitOps?**  
Push pipelines require cluster credentials and apply change; pull controllers run in cluster and reconcile from a source of truth, improving separation and drift control.

**Why not manually Helm rollback under GitOps?**  
The controller will reconcile back to Git. Update/revert the declared source or use an approved incident suspension and then reconcile.

## Practical exercise

Represent one app as a Kustomize base with Dev/Prod overlays, then as a small Helm chart. Render and diff both. Configure Flux in a sandbox, change the desired image digest through PR, create manual drift, and observe reconciliation. Practice a Git revert.

## Official references

- [GitOps-based deployments for AKS](https://learn.microsoft.com/azure/azure-arc/kubernetes/gitops-overview)
- [GitOps with Flux v2](https://learn.microsoft.com/azure/azure-arc/kubernetes/conceptual-gitops-flux2)
- [Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)
- [Helm chart best practices](https://helm.sh/docs/chart_best_practices/)

[Next: Readiness, Liveness, and Startup Probes →](08-readiness-liveness-and-startup-probes.md)
