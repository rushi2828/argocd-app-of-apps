
<img width="1024" height="1024" alt="ChatGPT Image Jul 27, 2025, 03_22_33 PM" src="https://github.com/user-attachments/assets/9fd3a0e9-0f8e-46b4-969f-609cf1f3eace" />

# argocd-app-of-apps

This repository demonstrates how to deploy applications using ArgoCD's App of Apps pattern with Helm charts. It includes the official Guestbook application and an additional sample app (2048 game). ArgoCD is installed via Helm, and GitOps is enabled for managing applications declaratively.

---

## 🧱 Project Structure

```
argocd-app-of-apps/
├── charts/
│   └── argocd/                  # ArgoCD Helm chart (optional overrides)
├── apps/
│   ├── guestbook-app.yaml       # ArgoCD app for Guestbook Helm chart
│   └── 2048-game-app.yaml       # ArgoCD app for 2048 game
├── secrets/
│   └── git-creds-secret.yaml    # (Optional) for private repo access
└── README.md
```

---

## 🚀 Getting Started

### 1. Create a KIND Cluster
```bash
kind create cluster --name argocd-demo
```

### 2. Create `argocd` Namespace
```bash
kubectl create namespace argocd
```

### 3. Build Helm Dependencies
```bash
helm dependency build charts/argocd
```

### 4. Install ArgoCD via Helm
```bash
helm install argocd charts/argocd/ -n argocd
```

### 5. Apply ArgoCD Guestbook App
```bash
kubectl -n argocd apply -f apps/guestbook-app.yaml
```

> 💡 If your repo is private, add the `git-creds-secret.yaml` to authenticate with GitHub.

---

## 🔐 ArgoCD Admin Access

### Get ArgoCD Admin Password
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```

### Port Forward ArgoCD UI
```bash
kubectl port-forward svc/argocd-server -n argocd 8090:443
```
Visit: [https://localhost:8090](https://localhost:8090)

---

## 🎮 Access the Guestbook App

### Port Forward Guestbook Service
```bash
kubectl port-forward svc/guestbook 80:80 -n guestbook
```

Visit: [http://localhost](http://localhost)

---

## 📸 Screenshots

### ArgoCD UI with Guestbook Application
![ArgoCD UI](https://github.com/user-attachments/assets/b9101d81-7289-4cc4-8d42-f7fec18ae685)

### Guestbook Application Details
![Guestbook in ArgoCD](https://github.com/user-attachments/assets/08cd178b-30b8-4d9d-b340-e4663b4aa713)

### Guestbook Web UI
![Guestbook Home](https://github.com/user-attachments/assets/2a984863-dffe-41fd-95d7-fc9374ef1682)

---

## 🧾 Notes
- App of Apps root definition can be placed under `root-app/` and applied for auto-syncing multiple applications.
- Each app (like guestbook) can point to a separate repo or Helm chart path.
- Use `CreateNamespace=true` in `syncOptions` of each child app if target namespace doesn't exist.

---

## 📎 Resources
- [ArgoCD Docs](https://argo-cd.readthedocs.io/)
- [Official Guestbook Helm Chart](https://github.com/argoproj/argocd-example-apps/tree/master/helm-guestbook)
- [ArgoCD Helm Chart](https://github.com/argoproj/argo-helm)

---

Happy GitOps-ing! 🚀
