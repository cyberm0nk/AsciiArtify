# PoC: Argo CD Deployment for AsciiArtify

## Goal

As part of the Proof of Concept (PoC) for AsciiArtify, we deployed a local Kubernetes cluster using the tool approved in the Concept phase (**k3d**) and installed **Argo CD**. The team now has access to the Argo CD graphical interface, which is ready for further MVP implementation.

---

## Steps

### 1. Create Kubernetes cluster (k3d)

#### Install k3d (if not already installed)

```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

#### Create cluster with server + agent nodes

```bash
k3d cluster create asciiartify \
  --servers 1 --agents 1 \
  --port "80:80@loadbalancer" \
  --port "443:443@loadbalancer"
```

#### Verify cluster is running

```bash
kubectl get nodes
```

### 2. Create namespace

```bash
kubectl create namespace argocd
```

### 3. Apply official Argo CD installation manifest

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

#### Check that pods are starting

```bash
kubectl -n argocd get pods
```

#### Port-forward Argo CD server service

```bash
kubectl -n argocd port-forward svc/argocd-server 8080:443
```

Open in browser 👉 https://localhost:8080

Username: admin
Password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
 -o jsonpath='{.data.password}' | base64 -d; echo
```

After the first login, please change the admin password in the UI.

## Result

Kubernetes cluster successfully created with k3d.
Argo CD installed in namespace argocd.
Team can access the Argo CD UI via https://localhost:8080 with the initial admin credentials.
