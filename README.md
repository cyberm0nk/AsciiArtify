# Concept: Local Kubernetes Options for AsciiArtify

## Introduction

AsciiArtify considers Kubernetes as the main platform for deployment and scaling of its ML-based ASCII-art product.  
For local development and proof-of-concept (PoC) testing, we evaluated three popular tools:

- **Minikube** – runs a single-node Kubernetes cluster locally using VM drivers or container runtimes.
- **kind (Kubernetes IN Docker)** – spins up Kubernetes clusters inside Docker containers, often used for testing and CI/CD.
- **k3d** – lightweight wrapper around Rancher’s k3s, running inside Docker containers, optimized for fast local clusters.

We also considered licensing risks of Docker Desktop and looked at **Podman** as an alternative container runtime.

---

## Characteristics

| Feature / Tool         | Minikube                          | kind                                  | k3d                                  |
| ---------------------- | --------------------------------- | ------------------------------------- | ------------------------------------ |
| OS / Arch support      | Linux, macOS, Windows             | Linux, macOS, Windows                 | Linux, macOS, Windows                |
| Container runtime      | Docker, Podman, containerd, CRI-O | Docker (Podman experimental)          | Docker (Podman experimental)         |
| Automation             | `minikube start`, addons          | Config YAML, scripting                | CLI (`k3d cluster create`), API      |
| Monitoring / Dashboard | Built-in dashboard, addons        | No dashboard, minimal features        | Lightweight, integrates with k3s     |
| Resource usage         | Medium (VM or container-based)    | Low / medium                          | Very low (optimized for PoC)         |
| CI/CD usage            | Less common                       | Very common (ephemeral test clusters) | Suitable, but less popular than kind |

---

## Advantages and Disadvantages

**Minikube**  
✅ Official Kubernetes project, rich documentation, many addons.  
✅ Easy to start, includes dashboard for cluster management.  
❌ Heavier on resources, slower startup compared to others.  
❌ Primarily single-node, not ideal for scalability tests.

**kind**  
✅ Excellent for CI/CD and automated testing.  
✅ Ephemeral clusters, reproducible environments.  
❌ Depends on Docker; networking can be slower.  
❌ Lacks user-friendly features for developers (no dashboard).

**k3d**  
✅ Very lightweight, fastest startup, resource-efficient.  
✅ Based on k3s, production-grade lightweight Kubernetes.  
✅ Great for PoC and developer laptops.  
❌ Smaller community compared to Minikube.  
❌ Fewer addons and integrations out-of-the-box.

---

## Demo: k3d + Hello World

Below is a simple demonstration of using **k3d** to create a Kubernetes cluster and deploy a Hello World application:

```bash
# Install k3d
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

# Create a cluster named asciiartify
k3d cluster create asciiartify --servers 1 --agents 1

# Verify cluster is running
kubectl get nodes

# Deploy a Hello World app (nginx)
kubectl create deployment hello --image=nginx

# Expose the app on a LoadBalancer
kubectl expose deployment hello --port=80 --type=LoadBalancer

# Check the service
kubectl get svc hello
```

Now open http://localhost:port in your browser — you should see the Nginx welcome page. 🎉

## Demo recording

Watch the terminal demo here: https://asciinema.org/a/ErrJKIiyrR59FHG3KybnMwc8A

## Conclusion

Minikube → Recommended for developers new to Kubernetes. Best choice if you want a built-in dashboard and a simple local single-node cluster.

kind → Recommended for CI/CD pipelines. Ideal for automated ephemeral clusters during testing.

k3d → Recommended for AsciiArtify PoC. Lightweight, fast, resource-efficient — perfectly suited for developer machines and quick experiments.
