# Kubernetes CICD Tutorial


---

## Creating Container Registry and GitOps PAT

>In GitHub, go to settings, Developer settings, Personal access tokens, Tokens (classic) and create a new two:
  >1. **Named:** `Container Registry Access`
    >**Permissions:** repo, workflow, write:packages
  >2. **Named:** `GitOps Repository Access`
    >**Permissions:**  repo

>Go to the kubernetes-cicd-tutorial repo, settings, Secrets and variables, Actions, New repository secret, then create 2 secrets:
  >**Name:** `CR_PAT`, 
  >**Secret:** Value of the Container Registry Access token
  >**Name:** `GITOPS_PAT`, 
  >**Secret:** Value of the GitOps Registry Access token

---

## Self-hosted runner

>Go to the kubernetes-cicd-tutorial repo, settings, Actions, Runners, New self-hosted runner:
>If you have done this before, just run this command:
```bash
cd actions-runner
./run.sh
```

---

## ArgoCD Application

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: grade-submission-api
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/ezzat223/grade-api-gitops.git
    targetRevision: HEAD
    path: .
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

## Creating Image Pull Secrets

To allow Kubernetes to pull images from GitHub Container Registry:

```yaml
kubectl create secret docker-registry ghcr-secret \\
  --docker-server=ghcr.io \\
  --docker-username=ezzat22 \\
  --docker-password=<YOUR_PAT> \\
  --namespace=default
```
