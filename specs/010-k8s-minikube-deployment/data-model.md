# Data Model: Phase IV: Local Kubernetes Deployment

## Container Entities

### Frontend Docker Image
- **Name**: todo-frontend:latest
- **Base**: Node.js LTS (multi-stage build)
- **Ports**: Exposes port 3000
- **Volumes**: None required
- **Environment Variables**:
  - NEXT_PUBLIC_API_BASE_URL (for backend API communication)
- **Health Check**: HTTP GET on /health endpoint
- **Dependencies**: package.json, package-lock.json, source code
- **Build Args**: None required

### Backend Docker Image
- **Name**: todo-backend:latest
- **Base**: python:3.11-slim
- **Ports**: Exposes port 8000
- **Volumes**: None required
- **Environment Variables**:
  - DATABASE_URL (PostgreSQL connection string)
  - BETTER_AUTH_SECRET (authentication secret)
  - COHERE_API_KEY (Cohere API key)
- **Health Check**: HTTP GET on /health endpoint
- **Dependencies**: requirements.txt, source code
- **Build Args**: None required

## Kubernetes Entities

### Helm Chart (todo-frontend)
- **Chart Name**: todo-frontend
- **Version**: 1.0.0
- **AppVersion**: 1.0.0
- **Description**: Helm chart for the Todo Chatbot frontend service
- **Maintainers**: [Team responsible for frontend]
- **Dependencies**: None
- **Values**:
  - image.repository: todo-frontend
  - image.tag: latest
  - image.pullPolicy: IfNotPresent
  - service.port: 3000
  - service.type: ClusterIP
  - replicaCount: 1
  - resources.limits.cpu: 500m
  - resources.limits.memory: 512Mi
  - resources.requests.cpu: 250m
  - resources.requests.memory: 256Mi

### Helm Chart (todo-backend)
- **Chart Name**: todo-backend
- **Version**: 1.0.0
- **AppVersion**: 1.0.0
- **Description**: Helm chart for the Todo Chatbot backend service
- **Maintainers**: [Team responsible for backend]
- **Dependencies**: None
- **Values**:
  - image.repository: todo-backend
  - image.tag: latest
  - image.pullPolicy: IfNotPresent
  - service.port: 8000
  - service.type: ClusterIP
  - replicaCount: 1
  - resources.limits.cpu: 500m
  - resources.limits.memory: 512Mi
  - resources.requests.cpu: 250m
  - resources.requests.memory: 256Mi
  - secrets.existingSecret: todo-chatbot-secrets

## Configuration Entities

### Frontend Environment Configuration
- **NEXT_PUBLIC_API_BASE_URL**: Base URL for backend API calls
- **NEXT_PUBLIC_APP_NAME**: Application name for display
- **NEXT_PUBLIC_DEFAULT_THEME**: Default UI theme (sky-blue)

### Backend Environment Configuration
- **DATABASE_URL**: Connection string for PostgreSQL database
- **BETTER_AUTH_SECRET**: Secret key for JWT token signing
- **COHERE_API_KEY**: API key for Cohere AI service
- **PORT**: Port number for the application (defaults to 8000)
- **LOG_LEVEL**: Logging level (defaults to INFO)

## Kubernetes Secrets

### todo-chatbot-secrets
- **Type**: Opaque
- **Data**:
  - COHERE_API_KEY: [base64 encoded value]
  - BETTER_AUTH_SECRET: [base64 encoded value]
  - DATABASE_URL: [base64 encoded value]

## Network Entities

### Internal Communication
- **Frontend to Backend**: HTTP requests from frontend service to backend service
- **Expected Endpoint**: http://todo-backend-service:8000/api/ for API calls
- **CORS Policy**: Allow requests from frontend service origin

### External Access
- **Frontend Service**: Exposed via ingress controller for user access
- **Backend Service**: Internal service, not directly exposed externally
- **Ingress Configuration**: Routes from external users to frontend service

## Deployment Entities

### Kubernetes Resources (Required)
- **Namespace**: todo-chatbot (isolated environment for the application)
- **Frontend Deployment**: Runs todo-frontend:latest image with configurable replicas
- **Frontend Service**: Exposes frontend deployment internally and via ingress
- **Backend Deployment**: Runs todo-backend:latest image with configurable replicas
- **Backend Service**: Exposes backend deployment internally for frontend access
- **Ingress**: Routes external traffic to frontend service with TLS termination
- **Secrets**: Stores sensitive environment variables (DB URL, auth secret, API key)
- **ConfigMap**: Stores non-sensitive configuration (if needed)