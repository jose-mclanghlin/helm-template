# Helm Template Platform

A reusable Helm template for standardized Kubernetes deployments with flexible secret management: External Secrets Operator integration and conventional Kubernetes secrets support.

## 🏗️ Structure

```
helm-template/
├── tf-charts/
│   ├── .helm/chart-deploy-base/        # Reusable base chart
│   └── tf-helm-charts/                 # Application charts
│       └── my-new-app-00/              # Example application
└── docs/                               # Documentation
```

## 🚀 Quick Start

### Prerequisites
- Helm 3.12+
- Kubernetes cluster with External Secrets Operator
- AWS IAM Role configured for IRSA

### Usage

```bash
# Navigate to application
cd tf-charts/tf-helm-charts/my-new-app-00

# Update dependencies
helm dependency update

# Deploy
helm upgrade --install my-app . \
  -n my-app \
  -f dev.yaml \
  --create-namespace
```

## 📦 Features

The base chart includes:
- **Deployment** with security contexts
- **Service** and **Ingress** 
- **ConfigMap** for configuration
- **Flexible Secret Management**:
  - **External Secrets** for AWS Secrets Manager integration
  - **Conventional Secrets** for standard Kubernetes secrets
- **HPA/VPA** for auto-scaling
- **PDB** for high availability

## 📝 Configuration

Minimal `dev.yaml` example with External Secrets:

```yaml
chart-deploy-base:
  namespace:
    enabled: true
    name: "my-app"
  
  deployment:
    name: "my-app"
    replicas: 1
    container:
      image: "your-registry/app:latest"
      ports:
        - containerPort: 8080
  
  service:
    port: 80
    targetPort: 8080
  
  # Option 1: External Secrets (AWS Secrets Manager)
  externalSecret:
    enabled: true
    data:
      - secretKey: "API_KEY"
        remoteKey: "my-app/api-key"
  
  # Option 2: Conventional Kubernetes Secret
  secret:
    enabled: false # true for enabled
    data:
      API_KEY: "your-api-key-value"
```

- **Helm 3.12+** installed
- **Kubectl** configured for your cluster

