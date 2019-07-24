# prow

Kubernetes prow for ServiceMesher

## Usage

```shell
# Check k8s cluster info.
kubectl cluster-info

# Create github Personal-Access-Token for ci-robot account.
echo f9bb09edaee3a52087d9dd19daf207c253dxxxxx > oauth-token
kubectl -n prow create secret generic oauth-token --from-file=oauth=./oauth-token

# Create webhook secret.
openssl rand -hex 20 > hmac-token
kubectl -n prow create secret generic hmac-token --from-file=hmac=./hmac-token

# Check k8s secrets.
kubectl -n prow get secret

# Deploy Prow to k8s.
kubectl apply -f starter.yaml

# Check prow deploy.
kubectl -n prow get all

# Update job and plugin configmaps.
kubectl create configmap config \
  --from-file=config.yaml=./config.yaml --dry-run -o yaml \
  | kubectl -n prow replace configmap config -f -

kubectl create configmap plugins \
  --from-file=plugins.yaml=./plugins.yaml --dry-run -o yaml \
  | kubectl -n prow replace configmap plugins -f -

# Check k8s configmaps.
kubectl -n prow get cm
kubectl -n prow get cm plugins -o yaml

# Wait for all pods status come to Running.
kubectl -n prow get po
```
