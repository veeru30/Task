# ✨ EKS Cluster with Argo CD & Nginx Deployment

This project provisions an AWS EKS cluster using Terraform and deploys a sample Nginx application using Argo CD.

---

## 📆 Steps to Provision the Cluster

### ✅ Required Components

- **Network**: VPC, Subnets, Route Table, Internet Gateway (`vpc.tf` in the `terraform/` folder)
- **IAM**: Role and Policies (`role.tf`)
- **EKS Cluster & Node Group**: (`eks.tf`)

### 📊 Terraform Commands

```bash
terraform init
terraform plan
terraform apply
```

> 📸 Screenshots of the Terraform process are available in the `terraform/` folder.

---

## 🚀 Argo CD Login Instructions

### 🔧 Installation

```bash
# Create namespace
kubectl create namespace argocd

# Add Helm repo and update
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

# Install Argo CD with LoadBalancer
helm install argocd argo/argo-cd \
  --namespace argocd \
  --create-namespace \
  --set server.service.type=LoadBalancer
```

### 🔐 Login to Argo CD UI

1. **Get LoadBalancer IP**

   ```bash
   kubectl get svc argocd-server -n argocd
   ```

2. **Default Credentials**:
   - **Username**: `admin`
   - **Password**:
     ```bash
     kubectl -n argocd get secret argocd-initial-admin-secret \
       -o jsonpath="{.data.password}" | base64 -d
     ```

3. **Deploy Nginx via Argo CD**:
   - Use `argo-cd.yaml` to deploy a sample Nginx app.
   - The manifest syncs with a linked GitHub repository.

> 📸 Screenshots of the Argo CD deployment are available in the `argo/` folder.

---

## 🌐 Public URL Access for Nginx

The deployed Nginx application is exposed via a LoadBalancer:

```
http://ae75b8f09dd7a40abbe7d661cc4e2813-409313706.ap-south-1.elb.amazonaws.com
```

---

## 🔍 Notes

- Ensure you have `kubectl`, `helm`, and `terraform` installed and configured.
