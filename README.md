# AKS + Ingress NGINX Controller + Cert Manager

## Versions

- Azure Kubernetes Services
- Ingress NGINX Controller
- Cert Manager
- Kubernetes Dashboard

## Deploy Azure Kubernetes Cluster

## Deploy Ingress NGINX Controller

```bash
helm install \
  ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.replicaCount=3
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml
```

## Deploy Cert Manager

```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true
kubectl apply -f ./cert-manager --namespace cert-manager
```

For troubleshot only, uninstall cert-manager
```bash
kubectl delete -f ./cert-manager --namespace cert-manager
helm delete cert-manager --namespace cert-manager
```

## Apply Let's Encrypt Cluster Issuer

Replace email with your email in /cert-manager/issuer.yaml line 8
```bash
kubectl apply -f ./cert-manager
```

## Deploy Kubenates Dashboard

Replace host with your host in /kubernetes-dashboard/default-value.yaml line 182 and 189

```bash
helm install \
  kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard \
  --namespace kubernetes-dashboard \
  --create-namespace \
  -f ./kubernetes-dashboard/default-value.yaml
```

## Deploy hello world app for validation

```bash
kubectl apply -f ./hello-world
```

For troubleshot only, uninstall cert-manager
```bash
kubectl delete -f ./hello-world
```

## Troubleshot

###


### SSL Cert not created

```bash
kubectl delete certificate tls-secret --namespace hello-world
kubectl get certificaterequest --all-namespaces
kubectl describe certificaterequest   tls-secret-cgnd9  --namespace hello-world
```
