# basic-chart Helm chart

This repository contains a Helm chart to deploy an application with configurable persistent storage, optional Ingress support, extra Kubernetes objects, and a Horizontal Pod Autoscaler (HPA).

**Key features**
- Configurable container port (`containerPort`) that the pod receives traffic on
- Configurable persistence (enable/disable, existing claim, size, mount path)
- Optional Ingress configuration via `ingress.*`
- ConfigMap and Secret environment variable injection via `configMap.*` and `secret.*`
- Extra manifest support via `extraObjects`
- HPA configuration (min/max replicas and CPU target)

**Files**
- Chart: `Chart.yaml`
- Default values: `values.yaml`
- Templates: `templates/*`

Quick install

Install with the default values:

```bash
helm install my-release .
```

Enable persistence and change the container port:

```bash
helm install my-release . --set persistence.enabled=true \
  --set containerPort=9090 --set persistence.size=5Gi
```

Enable Ingress and add an extra object:

```bash
helm install my-release . --set ingress.enabled=true \
  --set ingress.hosts[0].host=example.com \
  --set ingress.hosts[0].paths[0].path=/ \
  --set ingress.hosts[0].paths[0].pathType=ImplementationSpecific
```

Inject ConfigMap and Secret values as environment variables:

```bash
helm install my-release . --set configMap.enabled=true \
  --set configMap.data.LOG_LEVEL=debug \
  --set secret.enabled=true \
  --set secret.stringData.DB_PASSWORD=supersecret
```

Add an extra Kubernetes object through values:

```yaml
extraObjects:
  - apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-config
    data:
      key: value
```

Or use a values file `my-values.yaml`:

```yaml
persistence:
  enabled: true
  size: 5Gi
  mountPath: /data

containerPort: 9090

hpa:
  enabled: true
  minReplicas: 2
  maxReplicas: 6
  targetCPUUtilizationPercentage: 60
```

```bash
helm install my-release . -f my-values.yaml
```

Notes
- If `persistence.existingClaim` is set, the chart will use that PVC instead of creating one.
- The Service `port` (`service.port`) and the Pod `containerPort` (`containerPort`) are configurable independently; `targetPort` is wired to `containerPort` by default.
- The HPA resource uses CPU utilization by default and can be disabled via `hpa.enabled`.

Want me to run `helm lint` on the chart or add an example `my-values.yaml` file?
