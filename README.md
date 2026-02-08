# GitOps Repository

This repository contains the declarative configuration for our Kubernetes infrastructure and applications managed by ArgoCD.

## Repository Structure
```
.
├── apps/                          # ArgoCD Application CRs
│   ├── dev/                      # Development environment apps
│   ├── staging/                  # Staging environment apps
│   └── production/               # Production environment apps
├── manifests/                    # Kubernetes manifests
│   └── demo-app/
│       ├── base/                 # Base manifests (shared)
│       └── overlays/             # Environment-specific overlays
│           ├── dev/
│           ├── staging/
│           └── production/
└── infrastructure/               # Cluster infrastructure
    ├── namespaces/
    └── rbac/
```

## Environments

| Environment | Auto-Sync | Approval Required | Purpose |
|-------------|-----------|-------------------|---------|
| **dev** | ✅ Yes | ❌ No | Development and testing |
| **staging** | ✅ Yes | ❌ No | Pre-production validation |
| **production** | ❌ No | ✅ Yes | Production workloads |

## Workflow

1. Developer commits code to application repository
2. CI builds container image and pushes to ECR
3. CI updates image tag in `manifests/*/overlays/dev/kustomization.yaml`
4. ArgoCD detects change and auto-syncs to dev cluster
5. After testing, promote to staging (auto-sync)
6. After approval, promote to production (manual sync)

## GitOps Principles

- **Declarative:** Desired state defined in Git
- **Versioned:** All changes tracked via Git commits
- **Pulled:** ArgoCD pulls from Git (not pushed from CI/CD)
- **Continuously Reconciled:** ArgoCD ensures cluster matches Git

## Getting Started

See [docs/INSTALLATION.md](docs/INSTALLATION.md) for setup instructions.
