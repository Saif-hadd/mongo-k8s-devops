# MongoDB + Mongo Express on Kubernetes with GitOps

## 🎯 Overview
This project demonstrates a complete deployment of MongoDB and Mongo Express on Kubernetes with:
- Production-ready configurations
- RBAC security
- Network policies
- Horizontal Pod Autoscaling
- Helm charts for easy deployment
- GitOps with ArgoCD
- CI/CD with GitHub Actions

## 🏗️ Architecture
```
┌─────────────────┐
│   Ingress       │
└────────┬────────┘
         │
┌────────▼─────────────┐
│  Mongo Express (HPA) │
│    (2-5 replicas)    │
└────────┬─────────────┘
         │
┌────────▼──────────┐
│  MongoDB          │
│  (StatefulSet)    │
└───────────────────┘
```

## 🚀 Quick Start

### Prerequisites
- Kubernetes cluster (1.24+)
- kubectl configured
- Helm 3.x
- ArgoCD installed (optional for GitOps)

### Deploy with kubectl
```bash
# Create namespace
kubectl apply -f k8s/namespace.yaml

# Apply RBAC
kubectl apply -f k8s/rbac/

# Apply secrets and configs
kubectl apply -f k8s/secrets/
kubectl apply -f k8s/configmaps/

# Deploy MongoDB
kubectl apply -f k8s/mongodb/

# Deploy Mongo Express
kubectl apply -f k8s/mongo-express/

# Apply autoscaling
kubectl apply -f k8s/autoscaling/

# Apply network policies (optional)
kubectl apply -f k8s/network-policies/
```

### Deploy with Helm
```bash
helm install mongo-demo ./helm/mongo-demo
```

### Deploy with ArgoCD
```bash
kubectl apply -f argocd/application.yaml
```

## 🔐 Access Mongo Express

### Via NodePort
```bash
kubectl get svc -n mongo-demo mongo-express-service
# Access at: http://<NODE_IP>:30081
```

### Via Ingress
Add to `/etc/hosts`:
```
127.0.0.1 mongo-express.local
```
Access at: http://mongo-express.local

### Via Port Forward
```bash
kubectl port-forward -n mongo-demo svc/mongo-express-service 8081:8081
# Access at: http://localhost:8081
```

## 📊 Monitoring
```bash
# Check pods
kubectl get pods -n mongo-demo

# Check HPA status
kubectl get hpa -n mongo-demo

# View logs
kubectl logs -n mongo-demo -l app=mongodb
kubectl logs -n mongo-demo -l app=mongo-express
```

## 🔧 Configuration

Edit `helm/mongo-demo/values.yaml` to customize:
- Replica counts
- Resource limits
- Storage size
- Credentials
- Ingress settings

## 🛡️ Security Features
- RBAC with least privilege
- Network policies for traffic control
- Secrets for sensitive data
- Resource limits to prevent resource exhaustion

## 📝 License
MIT