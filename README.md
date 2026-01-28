# GitOps Demo - AKS with ArgoCD

This repository demonstrates a GitOps workflow for AKS using ArgoCD with a clear separation between Platform and Application teams.

## Folder Structure

```
gitops-demo/
├── platform/                    # Platform Team managed resources
│   ├── argocd-apps/            # ArgoCD Application definitions
│   │   └── platform-apps.yaml  # App-of-Apps for platform components
│   ├── traefik/                # Traefik Ingress Controller
│   │   ├── namespace.yaml
│   │   ├── rbac.yaml
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingressclass.yaml
│   └── namespaces/             # Application namespaces managed by platform
│       └── app-team-alpha.yaml
│
├── applications/               # Application Teams managed resources
│   └── team-alpha/            # Team Alpha's applications
│       ├── sample-app/        # Sample application
│       │   ├── namespace.yaml
│       │   ├── deployment.yaml
│       │   ├── service.yaml
│       │   └── ingress.yaml
│       └── argocd-app.yaml    # ArgoCD Application for team-alpha
│
└── bootstrap/                  # Initial ArgoCD setup
    └── root-app.yaml          # Root Application (App-of-Apps pattern)
```

## Getting Started

### Prerequisites
- AKS cluster with ArgoCD deployed
- `kubectl` configured to access your cluster
- Git repository accessible by ArgoCD

### Bootstrap the GitOps Setup

1. Update the `repoURL` in all ArgoCD Application manifests to point to your Git repository.

2. Apply the root application to bootstrap everything:
   ```bash
   kubectl apply -f bootstrap/root-app.yaml
   ```

3. ArgoCD will automatically sync and deploy:
   - Platform components (Traefik Ingress Controller)
   - Application team namespaces
   - Sample applications

### Accessing the Sample Application

After deployment, get the Traefik LoadBalancer IP:
```bash
kubectl get svc traefik -n traefik-system
```

Add an entry to your hosts file or use the IP directly:
```
<EXTERNAL-IP> sample-app.example.com
```

## Team Responsibilities

### Platform Team
- Manages `platform/` directory
- Deploys and maintains infrastructure components (Traefik, cert-manager, etc.)
- Creates namespaces for application teams
- Defines RBAC and network policies

### Application Teams
- Each team manages their folder under `applications/team-<name>/`
- Deploys applications within their assigned namespaces
- Creates Ingress resources using platform-provided IngressClass
