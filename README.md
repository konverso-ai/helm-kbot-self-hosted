# Helm Repo - Konverso
This is a Helm repo for installing kbot on your Kubernetes cluster.

> :warning: **Please note**: We take Konverso's security and our users' trust very seriously. If 
you believe you have found a security issue in our Helm chart, _please responsibly disclose_ 
by submitting a Kbot Security ticket to our [Support Portal](https://konverso.atlassian.net/servicedesk/customer/portal/10).

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