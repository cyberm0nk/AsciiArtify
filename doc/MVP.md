# MVP: Argo CD GitOps Demo for AsciiArtify

## Overview

As part of the AsciiArtify MVP, we successfully demonstrated a **GitOps workflow** using **Argo CD** deployed on a local Kubernetes cluster (k3d).  
The demo shows how a sample application can be automatically synchronized from a Git repository into the cluster, providing continuous delivery out-of-the-box.

---

## Demo

▶️ Watch the recording here:  
[![Watch the demo on YouTube](https://img.youtube.com/vi/xqB6OSAeLc4/0.jpg)](https://www.youtube.com/watch?v=xqB6OSAeLc4)

---

## Key Points

- **Argo CD UI** is accessible locally via `https://localhost:8080`.
- The **demo application** was deployed from the `https://github.com/den-vasyliev/go-demo-app` repository.
- Changes in the Git repository are automatically **detected and synchronized** by Argo CD.
- The application state in the cluster is kept in sync with the declared Git source.

---

## Result

This MVP confirms that:

- Argo CD is fully functional in our local Kubernetes setup.
- The GitOps workflow works as intended: pushing changes to Git automatically propagates them to the cluster.
