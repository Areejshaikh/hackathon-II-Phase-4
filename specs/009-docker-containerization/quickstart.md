# Quickstart Guide: Docker Containerization for Todo Chatbot App

## Prerequisites

- Docker Desktop installed and running
- Node.js v18+ (for local development, not required in containers)
- Python 3.11+ (for local development, not required in containers)
- Gordon (optional, Docker CLI will be used as fallback)

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

### 3. Prepare Environment Files

Copy the example environment files and update with your actual values:

```bash
# In the backend directory
cd backend
cp .env.example .env
# Edit .env with your actual values for DATABASE_URL, BETTER_AUTH_SECRET, COHERE_API_KEY
```

### 4. Build Docker Images

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

### 5. Run the Containers

#### Backend Container
```bash
cd backend
docker run -d \
  --name todo-backend \
  -p 8000:8000 \
  --env-file .env \
  todo-backend:latest
```

#### Frontend Container
```bash
cd frontend
docker run -d \
  --name todo-frontend \
  -p 3000:3000 \
  -e NEXT_PUBLIC_API_BASE_URL=http://localhost:8000 \
  todo-frontend:latest
```

### 6. Verify the Setup

Check if containers are running:
```bash
docker ps
```

Check container logs:
```bash
# Backend logs
docker logs todo-backend

# Frontend logs
docker logs todo-frontend
```

Access the application:
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000/docs

### 7. Stop and Clean Up

```bash
# Stop containers
docker stop todo-frontend todo-backend

# Remove containers
docker rm todo-frontend todo-backend
```

## Troubleshooting

### Common Issues

1. **Port Already in Use**
   - Error: "port is already allocated"
   - Solution: Stop other applications using ports 3000 or 8000, or use different ports

2. **Environment Variables Missing**
   - Error: Application crashes due to missing config
   - Solution: Ensure .env file contains all required variables

3. **Dependency Installation Failures**
   - Error: Packages fail to install during build
   - Solution: Check network connectivity and retry the build

4. **Container Fails to Start**
   - Check logs: `docker logs <container-name>`
   - Common causes: Missing environment variables, incorrect configuration

### Useful Commands

```bash
# View logs in real-time
docker logs -f <container-name>

# Execute commands inside running container
docker exec -it <container-name> /bin/sh

# Check container resource usage
docker stats <container-name>

# Remove unused Docker objects to free space
docker system prune
```

## Next Steps

Once the Docker containers are running successfully, proceed to the Kubernetes deployment phase using Minikube:

1. Install Minikube and kubectl
2. Start Minikube cluster
3. Apply Kubernetes manifests to deploy the application
4. Configure ingress to access the application externally