# Azure Kubernetes Service

[← Kubernetes Core Resources](05-kubernetes-core-resources.md) · [Chapter 12](README.md) · [Next: Helm, Kustomize, and GitOps →](07-helm-kustomize-and-gitops.md)

## Managed does not mean unmanaged by you

AKS provides a managed Kubernetes control plane. Microsoft and customers share responsibility. You still own workload configuration, identities/RBAC, node pools and upgrade choices, networking, policies, secrets, data protection, observability, capacity, backup/recovery, and application availability.

AKS offers Automatic and Standard operating modes with different degrees of preconfiguration/control. Select from current support, workload constraints, security responsibility, and required customization—not only initial convenience.

## Architecture decisions

- Region, availability zones, and failure domains.
- Private/public API server access and authorized networks.
- Azure CNI/network model, IP capacity, egress, DNS, and network policy.
- System and user node pools, VM sizes, taints/tolerations, autoscaling.
- Microsoft Entra integration, Azure/Kubernetes RBAC, and admin access.
- Managed identity and workload identity/OIDC.
- ACR pull integration.
- Policy/admission and Pod Security.
- Upgrade and node OS update channels/maintenance windows.
- Logs, metrics, managed Prometheus/Grafana/Application Insights as appropriate.
- Persistent data, backup, restore, and regional recovery.

IP exhaustion and egress/firewall dependencies are common production failures; capacity-plan both before cluster creation.

## Identity layers

Human/operator access uses Microsoft Entra ID and Kubernetes/Azure RBAC. The cluster/control-plane identity manages Azure resources required by AKS. Kubelet identity can pull from ACR. Workload identity lets Pods exchange service-account identity for Microsoft Entra tokens to access Azure services without embedded secrets.

Do not confuse these identities or give the workload the node/cluster identity.

## Upgrades

Kubernetes has a support lifecycle. Test control-plane/node and API-version upgrades in representative environments, detect removed APIs, use disruption budgets carefully, maintain capacity for surge, and validate add-ons/controllers. Node image/OS patching is separate from application image rebuilding.

Automatic channels reduce toil but still require compatibility testing, maintenance planning, and observability.

## Security and operations

Use least privilege, private networking where required, policy/admission, supported versions, nonprivileged workloads, image governance, network policies, secrets integration, Defender where applicable, and audit/diagnostic logs. Keep system workloads isolated from application pressure with system pools and resource reservations.

## Interview preparation

**What does Microsoft manage in AKS?**  
The Kubernetes control plane service; responsibility for workload, nodes/settings, network, identity, data, policy, upgrades choices, and operations remains shared/customer-heavy.

**Managed identity versus workload identity?**  
Managed identities represent Azure resources; AKS workload identity federates a Kubernetes service account to a Microsoft Entra application/managed identity for a specific workload.

**Why separate node pools?**  
To isolate system/application or workload classes, use different VM/OS/configuration, apply taints, scale independently, and limit blast radius.

## Practical exercise

Diagram every identity and network path in an AKS sandbox. Deploy a workload using its own service account/workload identity to read one allowed Azure resource. Deny broader access. Simulate a node drain and verify disruption/capacity behavior.

## Official references

- [AKS core concepts](https://learn.microsoft.com/azure/aks/core-aks-concepts)
- [Secure an AKS deployment](https://learn.microsoft.com/azure/aks/secure-aks)
- [AKS cluster security and upgrade practices](https://learn.microsoft.com/azure/aks/operator-best-practices-cluster-security)
- [AKS workload identity](https://learn.microsoft.com/azure/aks/workload-identity-overview)

[Next: Helm, Kustomize, and GitOps →](07-helm-kustomize-and-gitops.md)
