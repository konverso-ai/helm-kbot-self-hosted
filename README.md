# Helm Chart - Kbot self hosted
This is a Helm Chart for installing kbot on your Kubernetes cluster.

> :warning: **Please note**: We take Konverso's security and our users' trust very seriously. If 
you believe you have found a security issue in our Helm chart, _please responsibly disclose_ 
by submitting a Kbot Security ticket to our [Support Portal](https://konverso.atlassian.net/servicedesk/customer/portal/10).

## Prerequisites
A Kubernetes cluster at version >= 1.24

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
helm registry login konversoai.azurecr.io \
  --username $USER_NAME \
  --password $PASSWORD
```
3. Create Kubernetes image pull secret
```bash
kubectl create secret docker-registry konverso-registry-secret \
    --namespace kbot \
    --docker-server=konversoai.azurecr.io \
    --docker-username=xxx \
    --docker-password=xxx
```
4. Install the Helm Charts
```bash
helm install kbot-self-hosted oci://konversoai.azurecr.io/helm/kbot-self-hosted --version 0.1.0 --namespace kbot --create-namespace -f values.yaml
```

## Changelog
Can be found in both `CHANGELOG.md` file and in a release description.

-----

For more information, please refer to the [kbot official docs](https://support.konverso.ai/).