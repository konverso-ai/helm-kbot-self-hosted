# Helm Chart - Kbot self hosted
This is a Helm Chart for installing kbot on your Kubernetes cluster.

> :warning: **Please note**: We take Konverso's security and our users' trust very seriously. If 
you believe you have found a security issue in our Helm chart, _please responsibly disclose_ 
by submitting a Kbot Security ticket to our [Support Portal](https://konverso.atlassian.net/servicedesk/customer/portal/10).

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/kbot-self-hosted)](https://artifacthub.io/packages/search?repo=kbot-self-hosted)

## Prerequisites
- Kubernetes: `>= 1.24`
- Helm v3.0.0+ 
- [ClamAV](https://github.com/wiremind/wiremind-helm-charts/tree/main/charts/clamav).
- [Keda autoscaler](https://keda.sh).
- Persistent Volume/Storage Class
  - The microservice `api-files` will store data on a persistent volume in the cluster.
  - Must support Dynamic Provisioning and ReadWriteMany access mode.
  - The requested storage space is 10Gi by default.
  - The Storage Class should be set up with reclaimPolicy: Retain, to prevent data loss.
  - We recommend the current implementations for the major cloud providers: [AWS EFS CSI](https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html), [Azure Files](https://docs.microsoft.com/en-us/azure/aks/azure-files-dynamic-pv), [GCP Filestore](https://cloud.google.com/filestore/docs)
- PostgresSQL database >= 16
- Redis >= 7.0
- Sensitive secrets, create a k8s secret file called `kbot-secrets` that contains the following keys or use your own secret management.
  - master-api-key
  - oauth2-proxy-api-key
  - psql-kbot-password

## Installation
1. Request docker credentials from Konverso support
2. Add our Helm repository
```bash
helm repo add kbot-self-hosted https://konverso-ai.github.io/helm-kbot-self-hosted/
```
3. Create Kubernetes image pull secret
```bash
kubectl create secret docker-registry konverso-registry-secret \
    --namespace kbot \
    --docker-server=konversoai.azurecr.io \
    --docker-username=xxx \
    --docker-password=xxx
```
4. Install
```bash
helm install kbot kbot-self-hosted/kbot-self-hosted --version 0.1.0 --namespace kbot --create-namespace -f kbot-helm-values.yaml
```
5. Upgrade
```bash
helm upgrade kbot kbot-self-hosted/kbot-self-hosted --version 0.1.1 --namespace kbot --create-namespace -f kbot-helm-values.yaml
```

## Changelog
Can be found in both `CHANGELOG.md` file and in a release description.

-----

For more information, please refer to the [kbot official docs](https://support.konverso.ai/).

## Values - Global Configs
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| global.imagePullSecrets | list | `[]` |  |
| global.imageRegistry | string | `"konversoai.azurecr.io"` |  |
| global.logLevel | string | `"warning"` |  |
| global.namespace | string | `"kbot"` |  |

## Values - apiAuthAntivirus
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apiAuthAntivirus.autoscaling.cooldownPeriod | int | `300` |  |
| apiAuthAntivirus.autoscaling.cpuThreshold | string | `"60"` |  |
| apiAuthAntivirus.autoscaling.enabled | bool | `true` |  |
| apiAuthAntivirus.autoscaling.maxReplicas | int | `5` |  |
| apiAuthAntivirus.autoscaling.minReplicas | int | `1` |  |
| apiAuthAntivirus.autoscaling.pollingInterval | int | `15` |  |
| apiAuthAntivirus.enabled | bool | `true` |  |
| apiAuthAntivirus.extraAnnotations | object | `{}` |  |
| apiAuthAntivirus.extraLabels | object | `{}` |  |
| apiAuthAntivirus.image.tag | string | `"ff5b5707"` |  |
| apiAuthAntivirus.ingress.annotations | object | `{}` |  |
| apiAuthAntivirus.ingress.enabled | bool | `true` |  |
| apiAuthAntivirus.ingress.hostname | string | `""` |  |
| apiAuthAntivirus.ingress.ingressClassName | string | `""` |  |
| apiAuthAntivirus.ingress.labels | object | `{}` |  |
| apiAuthAntivirus.ingress.path | string | `"/"` |  |
| apiAuthAntivirus.ingress.pathType | string | `"Prefix"` |  |
| apiAuthAntivirus.ingress.tls.secretName | string | `""` |  |
| apiAuthAntivirus.podExtraAnnotations | object | `{}` |  |
| apiAuthAntivirus.podExtraLabels | object | `{}` |  |
| apiAuthAntivirus.resources.limits.cpu | string | `"800m"` |  |
| apiAuthAntivirus.resources.limits.memory | string | `"700Mi"` |  |
| apiAuthAntivirus.resources.requests.cpu | string | `"25m"` |  |
| apiAuthAntivirus.resources.requests.memory | string | `"300Mi"` |  |
| apiAuthAntivirus.service.annotations | object | `{}` |  |
| apiAuthAntivirus.tenantServiceUrl | string | `""` |  |

## Values - apiAuthentication
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apiAuthentication.autoscaling.cooldownPeriod | int | `300` |  |
| apiAuthentication.autoscaling.cpuThreshold | string | `"60"` |  |
| apiAuthentication.autoscaling.enabled | bool | `true` |  |
| apiAuthentication.autoscaling.maxReplicas | int | `5` |  |
| apiAuthentication.autoscaling.minReplicas | int | `1` |  |
| apiAuthentication.autoscaling.pollingInterval | int | `15` |  |
| apiAuthentication.enabled | bool | `true` |  |
| apiAuthentication.extraAnnotations | object | `{}` |  |
| apiAuthentication.extraLabels | object | `{}` |  |
| apiAuthentication.image.tag | string | `"7122aa0c"` |  |
| apiAuthentication.ingress.enabled | bool | `true` |  |
| apiAuthentication.ingress.host | string | `""` |  |
| apiAuthentication.ingress.ingressClassName | string | `""` |  |
| apiAuthentication.podExtraAnnotations | object | `{}` |  |
| apiAuthentication.podExtraLabels | object | `{}` |  |
| apiAuthentication.postgres.dbName | string | `""` |  |
| apiAuthentication.postgres.host | string | `""` |  |
| apiAuthentication.postgres.password.secretKey | string | `"psql-kbot-password"` |  |
| apiAuthentication.postgres.password.secretName | string | `"kbot-secrets"` |  |
| apiAuthentication.postgres.port | string | `"5432"` |  |
| apiAuthentication.postgres.user | string | `""` |  |
| apiAuthentication.resources.limits.cpu | string | `"600m"` |  |
| apiAuthentication.resources.limits.memory | string | `"500Mi"` |  |
| apiAuthentication.resources.requests.cpu | string | `"25m"` |  |
| apiAuthentication.resources.requests.memory | string | `"300Mi"` |  |
| apiAuthentication.service.annotations | object | `{}` |  |

## Values - apiAuthPdfConverter
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apiAuthPdfConverter.autoscaling.cooldownPeriod | int | `300` |  |
| apiAuthPdfConverter.autoscaling.cpuThreshold | string | `"60"` |  |
| apiAuthPdfConverter.autoscaling.enabled | bool | `true` |  |
| apiAuthPdfConverter.autoscaling.maxReplicas | int | `5` |  |
| apiAuthPdfConverter.autoscaling.minReplicas | int | `1` |  |
| apiAuthPdfConverter.autoscaling.pollingInterval | int | `15` |  |
| apiAuthPdfConverter.enabled | bool | `true` |  |
| apiAuthPdfConverter.extraAnnotations | object | `{}` |  |
| apiAuthPdfConverter.extraLabels | object | `{}` |  |
| apiAuthPdfConverter.image.tag | string | `"35a2731b"` |  |
| apiAuthPdfConverter.ingress.annotations | object | `{}` |  |
| apiAuthPdfConverter.ingress.enabled | bool | `true` |  |
| apiAuthPdfConverter.ingress.hostname | string | `""` |  |
| apiAuthPdfConverter.ingress.ingressClassName | string | `""` |  |
| apiAuthPdfConverter.ingress.labels | object | `{}` |  |
| apiAuthPdfConverter.ingress.path | string | `"/"` |  |
| apiAuthPdfConverter.ingress.pathType | string | `"Prefix"` |  |
| apiAuthPdfConverter.ingress.tls.secretName | string | `""` |  |
| apiAuthPdfConverter.podExtraAnnotations | object | `{}` |  |
| apiAuthPdfConverter.podExtraLabels | object | `{}` |  |
| apiAuthPdfConverter.pvc.enabled | bool | `true` |  |
| apiAuthPdfConverter.pvc.name | string | `"api-auth-pdf-converter-data"` |  |
| apiAuthPdfConverter.pvc.size | string | `"5Gi"` |  |
| apiAuthPdfConverter.pvc.storageClassName | string | `""` |  |
| apiAuthPdfConverter.resources.limits.cpu | string | `"75m"` |  |
| apiAuthPdfConverter.resources.limits.memory | string | `"350Mi"` |  |
| apiAuthPdfConverter.resources.requests.cpu | string | `"25m"` |  |
| apiAuthPdfConverter.resources.requests.memory | string | `"300Mi"` |  |
| apiAuthPdfConverter.service.annotations | object | `{}` |  |

## Values - apiFiles
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apiFiles.autoscaling.cooldownPeriod | int | `300` |  |
| apiFiles.autoscaling.cpuThreshold | string | `"60"` |  |
| apiFiles.autoscaling.enabled | bool | `true` |  |
| apiFiles.autoscaling.maxReplicas | int | `5` |  |
| apiFiles.autoscaling.minReplicas | int | `1` |  |
| apiFiles.autoscaling.pollingInterval | int | `10` |  |
| apiFiles.configmap.enabled | bool | `true` |  |
| apiFiles.configmap.name | string | `"nginx-config"` |  |
| apiFiles.enabled | bool | `true` |  |
| apiFiles.extraAnnotations | object | `{}` |  |
| apiFiles.extraLabels | object | `{}` |  |
| apiFiles.image.tag | string | `"0081b9a5"` |  |
| apiFiles.ingress.annotations | object | `{}` |  |
| apiFiles.ingress.enabled | bool | `true` |  |
| apiFiles.ingress.host | string | `""` |  |
| apiFiles.podExtraAnnotations | object | `{}` |  |
| apiFiles.podExtraLabels | object | `{}` |  |
| apiFiles.postgres.dbName | string | `""` |  |
| apiFiles.postgres.host | string | `""` |  |
| apiFiles.postgres.password.secretKey | string | `""` |  |
| apiFiles.postgres.password.secretName | string | `""` |  |
| apiFiles.postgres.port | string | `"5432"` |  |
| apiFiles.postgres.user | string | `""` |  |
| apiFiles.pvc.enabled | bool | `true` |  |
| apiFiles.pvc.name | string | `"files-data-storage"` |  |
| apiFiles.pvc.size | string | `"10Gi"` |  |
| apiFiles.pvc.storageClassName | string | `""` |  |
| apiFiles.resources.limits.cpu | string | `"300m"` |  |
| apiFiles.resources.limits.memory | string | `"500Mi"` |  |
| apiFiles.resources.requests.cpu | string | `"25m"` |  |
| apiFiles.resources.requests.memory | string | `"250Mi"` |  |
| apiFiles.service.annotations | object | `{}` |  |

## Values - apiGotenberg
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apiGotenberg.args[0] | string | `"gotenberg"` |  |
| apiGotenberg.args[1] | string | `"--api-disable-health-check-logging=true"` |  |
| apiGotenberg.args[2] | string | `"--api-port=3000"` |  |
| apiGotenberg.args[3] | string | `"--api-timeout=300s"` |  |
| apiGotenberg.args[4] | string | `"--chromium-auto-start=true"` |  |
| apiGotenberg.args[5] | string | `"--chromium-max-queue-size=15"` |  |
| apiGotenberg.args[6] | string | `"--gotenberg-graceful-shutdown-duration=300s"` |  |
| apiGotenberg.args[7] | string | `"--log-format=json"` |  |
| apiGotenberg.args[8] | string | `"--log-level=info"` |  |
| apiGotenberg.autoscaling.cooldownPeriod | int | `300` |  |
| apiGotenberg.autoscaling.cpuThreshold | string | `"60"` |  |
| apiGotenberg.autoscaling.enabled | bool | `true` |  |
| apiGotenberg.autoscaling.maxReplicas | int | `5` |  |
| apiGotenberg.autoscaling.minReplicas | int | `1` |  |
| apiGotenberg.autoscaling.pollingInterval | int | `30` |  |
| apiGotenberg.enabled | bool | `true` |  |
| apiGotenberg.extraAnnotations | object | `{}` |  |
| apiGotenberg.extraLabels | object | `{}` |  |
| apiGotenberg.image.tag | float | `8.21` |  |
| apiGotenberg.podExtraAnnotations | object | `{}` |  |
| apiGotenberg.podExtraLabels | object | `{}` |  |
| apiGotenberg.resources.limits.cpu | string | `"800m"` |  |
| apiGotenberg.resources.limits.memory | string | `"1.5Gi"` |  |
| apiGotenberg.resources.requests.cpu | string | `"200m"` |  |
| apiGotenberg.resources.requests.memory | string | `"400Mi"` |  |
| apiGotenberg.service.annotations | object | `{}` |  |

## Values - apiOauth2Proxy
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apiOauth2Proxy.autoscaling.cooldownPeriod | int | `300` |  |
| apiOauth2Proxy.autoscaling.cpuThreshold | string | `"60"` |  |
| apiOauth2Proxy.autoscaling.enabled | bool | `true` |  |
| apiOauth2Proxy.autoscaling.maxReplicas | int | `5` |  |
| apiOauth2Proxy.autoscaling.minReplicas | int | `1` |  |
| apiOauth2Proxy.autoscaling.pollingInterval | int | `10` |  |
| apiOauth2Proxy.enabled | bool | `false` |  |
| apiOauth2Proxy.extraAnnotations | object | `{}` |  |
| apiOauth2Proxy.extraLabels | object | `{}` |  |
| apiOauth2Proxy.image.tag | string | `"0fe52478"` |  |
| apiOauth2Proxy.ingress.annotations | object | `{}` |  |
| apiOauth2Proxy.ingress.enabled | bool | `true` |  |
| apiOauth2Proxy.ingress.hostname | string | `""` |  |
| apiOauth2Proxy.ingress.ingressClassName | string | `""` |  |
| apiOauth2Proxy.ingress.labels | object | `{}` |  |
| apiOauth2Proxy.ingress.path | string | `"/"` |  |
| apiOauth2Proxy.ingress.pathType | string | `"Prefix"` |  |
| apiOauth2Proxy.ingress.tls.secretName | string | `""` |  |
| apiOauth2Proxy.podExtraAnnotations | object | `{}` |  |
| apiOauth2Proxy.podExtraLabels | object | `{}` |  |
| apiOauth2Proxy.resources.limits.cpu | string | `"75m"` |  |
| apiOauth2Proxy.resources.limits.memory | string | `"200Mi"` |  |
| apiOauth2Proxy.resources.requests.cpu | string | `"25m"` |  |
| apiOauth2Proxy.resources.requests.memory | string | `"100Mi"` |  |
| apiOauth2Proxy.service.annotations | object | `{}` |  |