# helm-redis

Crossplane configuration that wraps the [Bitnami Redis](https://github.com/bitnami/charts/tree/main/bitnami/redis) Helm chart, providing a minimal, stable interface for deploying Redis on Kubernetes.

## Usage

### Minimal

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Redis
metadata:
  name: redis
  namespace: my-env
spec:
  clusterName: my-cluster
```

### With Replication and Metrics

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Redis
metadata:
  name: redis
  namespace: my-env
spec:
  clusterName: my-cluster

  labels:
    team: platform

  helm:
    namespace: redis
    values:
      architecture: replication
      auth:
        enabled: true
      replica:
        replicaCount: 3
      master:
        persistence:
          enabled: true
          size: 8Gi
      metrics:
        enabled: true
        serviceMonitor:
          enabled: true
```

## Configuration

| Field | Description | Default |
|-------|-------------|---------|
| `spec.clusterName` | Target cluster name (used as default providerConfigRef) | Required |
| `spec.labels` | Labels applied to all resources | `{}` |
| `spec.providerConfigRef.name` | Helm ProviderConfig name | `<clusterName>` |
| `spec.providerConfigRef.kind` | Helm ProviderConfig kind | `ProviderConfig` |
| `spec.helm.namespace` | Namespace for the Helm release | `redis` |
| `spec.helm.name` | Name of the Helm release | `redis` |
| `spec.helm.values` | Helm values (merged with defaults) | `{}` |
| `spec.helm.overrideAllValues` | Helm values that replace all defaults | `{}` |

## Chart Info

- **Chart**: redis
- **Repository**: https://charts.bitnami.com/bitnami
- **Version**: 24.1.0
