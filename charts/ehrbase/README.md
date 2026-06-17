# EHRbase Helm Chart

![Version: 0.3.0](https://img.shields.io/badge/Version-0.3.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 2.33.0](https://img.shields.io/badge/AppVersion-2.33.0-informational?style=flat-square)

A Helm chart for deploying EHRbase on Kubernetes.

> [!IMPORTANT]
> This chart is in early development and should not be used in production environments. It is intended for testing and
> development purposes only.

## Prerequisites

- [CloudNativePG](https://cloudnative-pg.io/) 1.29+ (if using the included PostgreSQL deployment)

## Getting Started

To install the chart:

```bash
helm install ehrbase oci://ghcr.io/tourbillon-health/helm-charts/ehrbase \
  --namespace ehrbase \
  --create-namespace
```

To upgrade an existing release:

```bash
helm upgrade ehrbase oci://ghcr.io/tourbillon-health/helm-charts/ehrbase \
  --namespace ehrbase
```

To uninstall the chart:

```bash
helm uninstall ehrbase --namespace ehrbase
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| autoscaling.enabled | bool | `false` |  |
| autoscaling.maxReplicas | int | `10` |  |
| autoscaling.minReplicas | int | `1` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| cnpg.cluster.initdb.database | string | `"ehrbase"` |  |
| cnpg.cluster.initdb.encoding | string | `"UTF-8"` |  |
| cnpg.cluster.initdb.owner | string | `"ehrbase"` |  |
| cnpg.cluster.initdb.postInitApplicationSQL[0] | string | `"CREATE SCHEMA IF NOT EXISTS ehr AUTHORIZATION ehrbase;"` |  |
| cnpg.cluster.initdb.postInitApplicationSQL[1] | string | `"CREATE SCHEMA IF NOT EXISTS ext AUTHORIZATION ehrbase;"` |  |
| cnpg.cluster.initdb.postInitApplicationSQL[2] | string | `"CREATE EXTENSION IF NOT EXISTS \"uuid-ossp\" SCHEMA ext;"` |  |
| cnpg.cluster.initdb.postInitApplicationSQL[3] | string | `"ALTER DATABASE ehrbase SET search_path TO ext;"` |  |
| cnpg.cluster.initdb.postInitApplicationSQL[4] | string | `"ALTER DATABASE ehrbase SET intervalstyle = 'iso_8601';"` |  |
| cnpg.cluster.instances | int | `1` |  |
| cnpg.cluster.storage.size | string | `"2Gi"` |  |
| cnpg.enabled | bool | `true` |  |
| cnpg.mode | string | `"standalone"` |  |
| cnpg.type | string | `"postgresql"` |  |
| cnpg.version.postgresql | string | `"18"` |  |
| config.basicAuth.adminPassword | string | `""` |  |
| config.basicAuth.adminUsername | string | `"ehrbase-admin"` |  |
| config.basicAuth.enabled | bool | `true` |  |
| config.basicAuth.existingSecret | string | `""` |  |
| config.basicAuth.password | string | `""` |  |
| config.basicAuth.username | string | `"ehrbase-user"` |  |
| config.enabled | bool | `true` |  |
| config.file | string | `"management:\n  endpoint:\n    health:\n      access: read_only\n  endpoints:\n    web:\n      access: public\n  server:\n    port: {{ .Values.service.ports.management }}\n\nplugin-manager:\n  enable: false\n\nserver:\n  port: {{ .Values.service.ports.http }}\n  servlet:\n    context-path: /\n\nspringdoc:\n  api-docs:\n    enabled: false\n  swagger-ui:\n    enabled: false\n"` |  |
| fullnameOverride | string | `""` |  |
| httpRoute | object | `{"annotations":{},"enabled":false,"hostnames":["ehrbase.local"],"parentRefs":[{"name":"gateway","sectionName":"http"}],"rules":[{"matches":[{"path":{"type":"PathPrefix","value":"/headers"}}]}]}` | Expose the service via gateway-api HTTPRoute Requires Gateway API resources and suitable controller installed within the cluster (see: https://gateway-api.sigs.k8s.io/guides/) |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ehrbase/ehrbase"` |  |
| image.tag | string | `""` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"ehrbase.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| initContainer.image.pullPolicy | string | `"IfNotPresent"` |  |
| initContainer.image.repository | string | `"busybox"` |  |
| initContainer.image.tag | string | `"stable"` |  |
| initContainer.securityContext.capabilities.drop[0] | string | `"ALL"` |  |
| initContainer.securityContext.readOnlyRootFilesystem | bool | `true` |  |
| initContainer.securityContext.runAsNonRoot | bool | `true` |  |
| initContainer.securityContext.runAsUser | int | `1000` |  |
| livenessProbe.httpGet.path | string | `"/management/health/liveness"` |  |
| livenessProbe.httpGet.port | string | `"management"` |  |
| livenessProbe.initialDelaySeconds | int | `30` |  |
| livenessProbe.timeoutSeconds | int | `3` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| podAnnotations | object | `{}` |  |
| podLabels | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| readinessProbe.httpGet.path | string | `"/management/health/readiness"` |  |
| readinessProbe.httpGet.port | string | `"management"` |  |
| readinessProbe.initialDelaySeconds | int | `30` |  |
| readinessProbe.timeoutSeconds | int | `3` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext.capabilities.drop[0] | string | `"ALL"` |  |
| securityContext.readOnlyRootFilesystem | bool | `false` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `1000` |  |
| service.ports.http | int | `8080` |  |
| service.ports.management | int | `9000` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.automount | bool | `true` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `""` |  |
| tolerations | list | `[]` |  |
| volumeMounts | list | `[]` |  |
| volumes | list | `[]` |  |