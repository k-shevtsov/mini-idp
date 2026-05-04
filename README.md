# 🚀 Mini IDP — Internal Developer Portal

A self-service Kubernetes deployment portal built with **Port.io** and **GitHub Actions**.  
Developers can deploy services to Kubernetes through a UI form — no `kubectl` required.

## Architecture
```
Developer → Port.io UI → GitHub Actions → Kubernetes Cluster (kind)
```

## Features

-  **Software Catalog** — real-time visibility into K8s Workloads, Pods, Nodes, Namespaces
-  **Self-service Deployments** — deploy any service via UI form with guardrails
-  **Production Readiness Scorecards** — track service maturity (owner, repo, on-call)
-  **Real-time status reporting** — deployment logs streamed back to Port UI
-  **GitHub Integration** — repositories and PRs visible in the catalog

## Stack

| Tool | Purpose |
|---|---|
| [Port.io](https://getport.io) | Internal Developer Portal |
| [kind](https://kind.sigs.k8s.io/) | Local Kubernetes cluster |
| [GitHub Actions](https://github.com/features/actions) | Automation backend |
| [Helm](https://helm.sh/) | Kubernetes package manager |
| [kubectl](https://kubernetes.io/docs/reference/kubectl/) | Cluster management |

## Project Structure
```
mini-idp/
├── .github/
│   └── workflows/
│       └── deploy-service.yml   # Self-service deployment workflow
├── k8s/
│   └── apps/
│       └── demo-app.yaml        # Demo application manifest
└── kind-cluster.yaml            # Local K8s cluster config
```

## How It Works

1. Developer opens **Port → Self-service → 🚀 Deploy Service**
2. Fills in the form: service name, Docker image, replicas, namespace, environment
3. Port triggers **GitHub Actions** workflow via GitHub App
4. Workflow generates a Kubernetes manifest and applies it
5. Deployment status is reported back to Port in **real-time**

## ⚙️ Local Setup

### Prerequisites
- Docker
- Git

### Installation
```bash
# 1. Clone the repository
git clone https://github.com/k-shevtsov/mini-idp.git
cd mini-idp

# 2. Install tools
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64
chmod +x ./kind && sudo mv ./kind /usr/local/bin/kind

curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# 3. Create the cluster
kind create cluster --config kind-cluster.yaml

# 4. Deploy demo app
kubectl apply -f k8s/apps/demo-app.yaml

# 5. Install Port Kubernetes Exporter
helm repo add port-labs https://port-labs.github.io/helm-charts
helm repo update
helm install port-k8s-exporter port-labs/port-k8s-exporter \
  --namespace port-agent --create-namespace \
  --set secret.secrets.portClientId=YOUR_CLIENT_ID \
  --set secret.secrets.portClientSecret=YOUR_CLIENT_SECRET \
  --set "extraEnv[0].name=CLUSTER_NAME" \
  --set "extraEnv[0].value=mini-idp"
```

## Screenshots

### Software Catalog — K8s Workloads
> Real-time view of all workloads running in the cluster
<img width="1215" height="359" alt="Screenshot from 2026-03-17 22-45-36" src="https://github.com/user-attachments/assets/a4a0c74c-dabf-4088-9bd1-13dd93c3f752" />

### Self-service — Deploy Service
> One-click deployment form with built-in guardrails
<img width="621" height="584" alt="Screenshot from 2026-03-17 22-53-20" src="https://github.com/user-attachments/assets/b1a4de50-b754-4d8b-b41d-4d4b80e23459" />

### Deployment Log Stream
> Real-time feedback directly in Port UI
<img width="1398" height="552" alt="Screenshot from 2026-03-17 22-49-55" src="https://github.com/user-attachments/assets/a8b17c7e-ac2c-4c08-b00b-7c4e409dec4f" />

## Evolution

! This project evolved into [**ai-enhanced-idp**](https://github.com/k-shevtsov/ai-enhanced-idp) with:

- [x] ArgoCD GitOps — App-of-Apps pattern
- [x] Security scorecards — Claude AI agent, Bronze/Silver/Gold levels
- [ ] Crossplane — planned for cloud resource provisioning
- [ ] Slack notifications — planned

## 👤 Author

**Kostiantyn Shevtsov** — DevOps Engineer  
[GitHub](https://github.com/k-shevtsov)
[LinkedIn](https://www.linkedin.com/in/kostiantyn-shevtsov)

