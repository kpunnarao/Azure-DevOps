# Kubernetes Core Resources

[← Azure Container Registry](04-azure-container-registry.md) · [Chapter 12](README.md) · [Next: Azure Kubernetes Service →](06-azure-kubernetes-service.md)

## Reconciliation model

Kubernetes stores desired objects in its API. Controllers continually reconcile actual state toward the specification. The scheduler assigns unscheduled Pods to nodes; kubelets and the container runtime run them. Controllers are asynchronous, so “object accepted” is not “application healthy.”

## Essential resources

- **Pod:** smallest scheduled unit; one or more tightly coupled containers.
- **Deployment:** manages stateless ReplicaSets/Pods and rolling updates.
- **StatefulSet:** stable identity/ordered behavior for stateful workloads.
- **DaemonSet:** one Pod per eligible node.
- **Job/CronJob:** finite or scheduled work.
- **Service:** stable virtual endpoint selecting Pods by labels.
- **ConfigMap:** non-secret configuration.
- **Secret:** sensitive Kubernetes object; base64 is encoding, not encryption.
- **Ingress/Gateway:** HTTP/network traffic routing through a controller/implementation.
- **PersistentVolume/PersistentVolumeClaim/StorageClass:** storage supply and claim.
- **Namespace:** organizational/security boundary component, not complete isolation.
- **ServiceAccount:** workload's Kubernetes identity.

## Deployment and Service example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: orders
spec:
  replicas: 3
  selector:
    matchLabels:
      app: orders
  template:
    metadata:
      labels:
        app: orders
    spec:
      containers:
      - name: api
        image: contoso.azurecr.io/orders@sha256:REPLACE
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            memory: 256Mi
---
apiVersion: v1
kind: Service
metadata:
  name: orders
spec:
  selector:
    app: orders
  ports:
  - port: 80
    targetPort: 8080
```

If labels do not match the Service selector, Pods can be healthy yet receive no traffic.

## Scheduling and availability

Requests drive scheduling and resource guarantees; limits constrain usage. Poor values cause pending Pods, throttling, or out-of-memory termination. Use topology spread/anti-affinity, disruption budgets, multiple replicas, and graceful termination to preserve availability, but understand node capacity and failure domains.

## Configuration and secrets

ConfigMap/Secret values consumed as environment variables usually do not update the process automatically. Mounted volumes may update, but the application must reload safely. Use an external secret provider/workload identity for stronger secret lifecycle where appropriate. Encrypt Kubernetes secrets at rest and restrict RBAC.

## Diagnostics

Inspect desired object, status/conditions, events, ReplicaSet, Pods, scheduling, image pull, container state/restarts, probes, logs, endpoints/EndpointSlices, DNS/network policy, and metrics. Events often explain the first failure.

## Interview preparation

**Deployment versus StatefulSet?**  
Deployment for interchangeable stateless replicas; StatefulSet for stable identity, ordering, and persistent-volume association.

**Service versus Ingress?**  
Service gives stable internal access/load balancing; Ingress/Gateway exposes/routs higher-level traffic via a controller.

**Why can a Running Pod be unavailable?**  
Running describes container state, not readiness, Service selection, routing, dependency health, or correct responses.

## Practical exercise

Deploy three Pods and a Service. Break the selector, resource request, image digest, and ConfigMap reference one at a time. Diagnose each using conditions/events/endpoints rather than redeploying blindly.

## Official references

- [Kubernetes concepts](https://kubernetes.io/docs/concepts/)
- [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Services](https://kubernetes.io/docs/concepts/services-networking/service/)

[Next: Azure Kubernetes Service →](06-azure-kubernetes-service.md)
