# basic-chart Helm chart

This repository contains a Helm chart to deploy an application with configurable persistent storage and a Horizontal Pod Autoscaler (HPA).

**Key features**
- Configurable container port (`containerPort`) that the pod receives traffic on
- Configurable persistence (enable/disable, existing claim, size, mount path)
- HPA configuration (min/max replicas and CPU target)

**Files**
- Chart: `chart/Chart.yaml`
- Default values: `chart/values.yaml`
- Templates: `chart/templates/*`

Quick install

Install with the default values:

```bash
helm install my-release ./chart
```

Enable persistence and change the container port:

```bash
helm install my-release ./chart --set persistence.enabled=true \
  --set containerPort=9090 --set persistence.size=5Gi
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
helm install my-release ./chart -f my-values.yaml
```

Notes
- If `persistence.existingClaim` is set, the chart will use that PVC instead of creating one.
- The Service `port` (`service.port`) and the Pod `containerPort` (`containerPort`) are configurable independently; `targetPort` is wired to `containerPort` by default.
- The HPA resource uses CPU utilization by default and can be disabled via `hpa.enabled`.

Want me to run `helm lint` on the chart or add an example `my-values.yaml` file? 
