# Quickstart Guide: Phase IV: Local Kubernetes Deployment

## Prerequisites

- Docker Desktop installed and running
- Minikube installed (v1.25 or later)
- kubectl installed
- Helm installed (v3.x)
- kubectl-ai installed (optional, for AI-assisted operations)
- kagent installed (optional, for AI-assisted operations)
- Gordon (Docker AI) installed (optional, for AI-assisted Docker operations)

## Setup Instructions

### 1. Clone the Repository
```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Navigate to the Project Directory
```bash
cd D:\phase_4\Hackathon-II-Phase-3
```

### 3. Start Minikube Cluster
```bash
# Start Minikube with Docker driver and appropriate resources
minikube start --driver=docker --memory=4096 --cpus=2

# Verify cluster is running
kubectl cluster-info
```

### 4. Prepare Environment Files

Copy the example environment files and update with your actual values:

```bash
# In the backend directory
cd backend
cp .env.example .env
# Edit .env with your actual values for DATABASE_URL, BETTER_AUTH_SECRET, COHERE_API_KEY
```

### 5. Build Docker Images

#### Using Gordon (AI Docker Assistant - Recommended)
```bash
# If Gordon is available, use it to generate optimized Dockerfiles
# Gordon commands would be provided by the AI assistant
```

#### Using Standard Docker (Fallback)
```bash
# Build frontend image
cd ../frontend
docker build -t todo-frontend:latest .

# Build backend image
cd ../backend
docker build -t todo-backend:latest .
```

### 6. Create Kubernetes Secrets

```bash
# Create a Kubernetes secret with your environment variables
kubectl create namespace todo-chatbot

kubectl create secret generic todo-chatbot-secrets \
  --namespace=todo-chatbot \
  --from-literal=COHERE_API_KEY=<your-cohere-api-key> \
  --from-literal=BETTER_AUTH_SECRET=<your-better-auth-secret> \
  --from-literal=DATABASE_URL=<your-database-url>
```

### 7. Deploy Using Helm Charts

```bash
# Navigate to the charts directory
cd ../k8s/charts

# Install backend chart first
helm install todo-backend ./todo-backend --namespace=todo-chatbot --create-namespace

# Install frontend chart
helm install todo-frontend ./todo-frontend --namespace=todo-chatbot
```

### 8. Verify the Setup

Check if pods are running:
```bash
kubectl get pods --namespace=todo-chatbot
```

Check if services are available:
```bash
kubectl get services --namespace=todo-chatbot
```

Access the application:
```bash
# Get the frontend service URL
minikube service todo-frontend-service --namespace=todo-chatbot --url
```

### 9. AI-Assisted Operations

#### Using kubectl-ai
```bash
# Scale backend to 3 replicas
kubectl-ai "scale todo-backend-deployment to 3 replicas in todo-chatbot namespace"

# Check why pods are failing
kubectl-ai "check why pods are failing in todo-chatbot namespace"

# Get cluster status
kubectl-ai "get status of all resources in todo-chatbot namespace"
```

#### Using kagent
```bash
# Analyze cluster health
kagent "analyze cluster health for todo-chatbot namespace"
```

### 10. Troubleshooting

#### Common Issues

1. **Minikube Not Starting**
   - Error: "The minikube VM is currently in the "Paused" state"
   - Solution: Run `minikube delete` and then `minikube start`

2. **Pods in CrashLoopBackOff**
   - Check logs: `kubectl logs <pod-name> --namespace=todo-chatbot`
   - Common causes: Missing environment variables, incorrect database URL, invalid API keys

3. **Services Not Accessible**
   - Check if pods are running: `kubectl get pods --namespace=todo-chatbot`
   - Check service configuration: `kubectl describe service <service-name> --namespace=todo-chatbot`

4. **ImagePullBackOff Error**
   - Error: Kubernetes can't pull the Docker images
   - Solution: Load images into Minikube: `minikube image load todo-frontend:latest`

#### Useful Commands

```bash
# View logs in real-time
kubectl logs -f <pod-name> --namespace=todo-chatbot

# Execute commands inside running pod
kubectl exec -it <pod-name> --namespace=todo-chatbot -- /bin/sh

# Check resource usage
kubectl top pods --namespace=todo-chatbot

# Check all resources in the namespace
kubectl get all --namespace=todo-chatbot

# Port forward for direct access (for debugging)
kubectl port-forward svc/todo-frontend-service 3000:3000 --namespace=todo-chatbot
kubectl port-forward svc/todo-backend-service 8000:8000 --namespace=todo-chatbot
```

### 11. Cleanup

```bash
# Uninstall Helm releases
helm uninstall todo-frontend --namespace=todo-chatbot
helm uninstall todo-backend --namespace=todo-chatbot

# Delete namespace
kubectl delete namespace todo-chatbot

# Stop Minikube
minikube stop

# Optionally delete the Minikube VM
minikube delete
```

## Next Steps

Once the Kubernetes deployment is working successfully, you can:
1. Scale the backend service to multiple replicas
2. Configure ingress for proper domain access
3. Set up monitoring and logging
4. Implement CI/CD pipelines for automated deployments