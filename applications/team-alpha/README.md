# Team Alpha Applications

This directory contains all applications managed by Team Alpha.

## Structure

```
team-alpha/
├── sample-app/          # Sample web application
│   ├── deployment.yaml  # Application deployment
│   ├── service.yaml     # ClusterIP service
│   ├── ingress.yaml     # Traefik ingress configuration
│   └── namespace.yaml   # Namespace reference (managed by platform)
└── README.md
```

## Adding New Applications

1. Create a new folder for your application under `team-alpha/`
2. Add your Kubernetes manifests
3. Request the Platform team to add an ArgoCD Application for your new app
4. Commit and push - ArgoCD will auto-sync

## Guidelines

- All applications must be deployed in the `team-alpha` namespace
- Use the `traefik` IngressClass for ingress resources
- Follow resource limits defined by the Platform team
- Use labels: `app: <app-name>` and `team: alpha`

## Accessing Applications

After deployment, your application will be accessible via the Traefik ingress.
Update the `host` field in your ingress to match your DNS configuration.
