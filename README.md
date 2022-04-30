# AKS + Ingress NGINX Controller + Cert Manager

## Versions

- Azure Kuberetes Services
- Ingress NGINX Controller
- Cert Manager
- Kuberetes Dashboard

## Steps

1. Deploy Azure Kubernetes Cluster

2. Deploy Ingress NGINX Controller

```
helm install \
  ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.replicaCount=3
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml
```

3. Deploy Cert Manager

```
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true \
  --set ingressShim.defaultIssuerName="letsencrypt-prod" \
  --set ingressShim.defaultIssuerKind="ClusterIssuer"

```

4. Apply Let's Encrypt Cluster Issuer

Replace email with your email in /cert-manager/issuer.yaml line 8
```
kubectl apply -f ./cert-manager
```

5. Deploy Kubenates Dashboard

Replace host with your host in /kubernetes-dashboard/default-value.yaml line 182 and 189
```
helm install \
  kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard \
  --namespace kubernetes-dashboard \
  --create-namespace \
  -f ./kubernetes-dashboard/default-value.yaml
```
