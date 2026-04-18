# helm-charts

Shared Helm charts for Primetime application deployments.

## `flask-service`

`charts/flask-service` is the standard chart for simple Flask-style HTTP services.
It owns the deployment shape while application repos provide image and environment
configuration.

Runtime contract:

- app listens on port `8080` by default
- `GET /healthz` is used for liveness
- `GET /readyz` is used for readiness

## Local Validation

Lint the chart:

```bash
helm lint charts/flask-service
```

Render the chart with inline values:

```bash
helm template demo-service-api charts/flask-service \
  --set image.repository=ghcr.io/primetime-dev/demo-service-api \
  --set image.tag=latest
```

Render with the example values file:

```bash
helm template demo-service-api charts/flask-service \
  -f examples/flask-service-values.yaml
```

## Publish Flow

Pushes to `main` publish the chart to GHCR as an OCI artifact. The chart version in
`charts/flask-service/Chart.yaml` is the release source of truth.

## Consumption

```bash
helm upgrade --install demo-service-api oci://ghcr.io/primetime-dev/charts/flask-service \
  --version 0.1.0 \
  -f values.yaml
```

The chart repository is public, so consumers do not need to log in to GHCR to pull it.

`values.yaml` should define the image repository, image tag, ingress host, and any
required environment variables.
