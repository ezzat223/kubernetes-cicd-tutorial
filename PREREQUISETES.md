# Install ArgoCD

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kubectl create namespace argocd
helm install argocd argo/argo-cd --namespace argocd
```

---

## Access ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:80
```

---

## Retrieve Credentials

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
