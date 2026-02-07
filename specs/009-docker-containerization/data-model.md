# Data Model: Docker Containerization for Todo Chatbot App

## Container Entities

### Frontend Docker Image
- **Name**: todo-frontend:latest
- **Base**: Node.js LTS (multi-stage build)
- **Ports**: Exposes port 3000
- **Volumes**: None required
- **Environment Variables**: 
  - NEXT_PUBLIC_API_BASE_URL (for backend API communication)
- **Health Check**: HTTP GET on /api/health
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
- **Health Check**: HTTP GET on /health
- **Dependencies**: requirements.txt, source code
- **Build Args**: None required

## Configuration Entities

### Frontend Environment Configuration
- **NEXT_PUBLIC_API_BASE_URL**: Base URL for backend API calls
- **NEXT_PUBLIC_APP_NAME**: Application name for display
- **NEXT_PUBLIC_DEFAULT_THEME**: Default UI theme

### Backend Environment Configuration
- **DATABASE_URL**: Connection string for PostgreSQL database
- **BETTER_AUTH_SECRET**: Secret key for JWT token signing
- **COHERE_API_KEY**: API key for Cohere AI service
- **PORT**: Port number for the application (defaults to 8000)
- **LOG_LEVEL**: Logging level (defaults to INFO)

## Network Entities

### Internal Communication
- **Frontend to Backend**: HTTP requests from frontend container to backend service
- **Expected Endpoint**: http://backend-service:8000/api/ for API calls
- **CORS Policy**: Allow requests from frontend service origin

### External Access
- **Frontend Service**: Exposed on port 3000 for user access
- **Backend Service**: Exposed on port 8000 for API access (may be internal only)
- **Ingress Configuration**: Routes from external users to frontend service

## Deployment Entities

### Kubernetes Resources (Future)
- **Frontend Deployment**: Runs todo-frontend:latest image
- **Frontend Service**: Exposes frontend deployment internally
- **Backend Deployment**: Runs todo-backend:latest image
- **Backend Service**: Exposes backend deployment internally
- **Ingress**: Routes external traffic to frontend service
- **Secrets**: Stores sensitive environment variables (DB URL, auth secret, API key)