# # EKS Config Settings

Comprehensive yet concise configuration and scripts for provisioning AWS EKS clusters and managing Kubernetes RBAC resources.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Examples](#examples)
- [Repository Structure](#repository-structure)
- [Contributing](#contributing)
- [License](#license)

## Features
- AWS IAM user and policy setup for EKS
- Example Kubernetes RBAC YAMLs
- Quick reference for service account token generation

# Prerequisites

Ensure you have the following installed and configured:
- AWS CLI
- kubectl
- eksctl

## Quick Start

1. **Create AWS IAM User & Attach Policies**
   - Attach required EKS and EC2 policies, plus a custom policy for `eks:*` actions.

2. **Create EKS Cluster & Nodegroup**
   - Use `eksctl` to provision your cluster and nodegroup.

3. **Kubernetes RBAC Setup**
   - Example ServiceAccount, Role, and RoleBinding for namespace `webapps`:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
      - ""
      - apps
      - autoscaling
      - batch
      - extensions
      - policy
      - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - pods
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role
subjects:
- namespace: webapps
  kind: ServiceAccount
  name: jenkins
```

4. **Generate Service Account Token**
   - See [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin) for instructions.

## Repository Structure
```text
LICENSE
Steps-eks.md         # Detailed EKS setup steps and policies
Policies.png         # Visual guide for IAM policies
README.md            # This guide
```

## Contributing
Contributions, issues, and feature requests are welcome! Please open a GitHub issue or submit a pull request.

## License
This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
