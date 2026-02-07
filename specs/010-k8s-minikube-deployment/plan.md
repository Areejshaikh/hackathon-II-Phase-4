# Implementation Plan: Phase IV: Local Kubernetes Deployment

**Branch**: `010-k8s-minikube-deployment` | **Date**: 2026-02-07 | **Spec**: [link](spec.md)
**Input**: Feature specification from `/specs/010-k8s-minikube-deployment/spec.md`

**Note**: This template is filled in by the `/sp.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

This plan outlines the deployment of the existing Todo Chatbot application (frontend Next.js with custom UI & sky-blue theme, backend FastAPI with Cohere API) to a local Kubernetes cluster (Minikube) using Docker containerization and Helm charts. The approach involves containerizing both frontend and backend applications, creating separate Helm charts for each service, deploying to a local Minikube cluster with docker driver, and implementing AI-assisted operations using Gordon, kubectl-ai, and kagent. The plan ensures all Phase III features continue to work identically while providing a cloud-native, zero-cost, beginner-friendly deployment solution.

## Technical Context

**Language/Version**: JavaScript/TypeScript (Node.js v18+ for Next.js frontend), Python 3.11 (for FastAPI backend)
**Primary Dependencies**: Next.js 14+, TypeScript, Tailwind CSS, Framer Motion (frontend), FastAPI, SQLModel, PostgreSQL, Cohere (backend)
**Storage**: PostgreSQL database (Neon DB), accessed via SQLModel ORM
**Testing**: Jest, React Testing Library (frontend), pytest (backend)
**Target Platform**: Docker containers running on local Kubernetes (Minikube) with Helm Charts
**Project Type**: Web application (frontend Next.js, backend FastAPI)
**Performance Goals**: Docker images build in under 10 minutes, containers start within 60 seconds, <500MB RAM usage per container, 99% uptime in Kubernetes
**Constraints**: Must maintain all existing functionality (login, tasks, Cohere chatbot), secure handling of environment variables (no hardcoded secrets), multi-stage builds for frontend optimization, Kubernetes-native deployment approach
**Scale/Scope**: Single application with separate frontend and backend services, designed for local Kubernetes deployment with potential for scaling to 3+ backend replicas

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### Compliance Verification:
- ✅ **Local Only**: Deployment will run on local Minikube cluster, no cloud dependencies
- ✅ **Zero-Cost & Beginner-Friendly**: Using Minikube with driver=docker, low resource requirements
- ✅ **Existing App Integration**: Maintaining all Phase III functionality (Cohere chatbot, custom UI, JWT auth)
- ✅ **Gordon Priority**: Will attempt to use Gordon (Docker AI) for builds, with standard Docker CLI as fallback
- ✅ **Secrets Security**: Environment variables (COHERE_API_KEY, BETTER_AUTH_SECRET, DATABASE_URL) will be handled securely via Kubernetes Secrets
- ✅ **No Breakage**: Ensuring all Phase III features (login, tasks, chat) continue to work post-Kubernetes deployment
- ✅ **Spec-Driven**: Following SpecKit methodology for deployment automation
- ✅ **Error Handling**: Including troubleshooting commands at each step
- ✅ **Specs Reference**: Referencing @specs/... throughout implementation
- ✅ **Tech Lock Compliance**: Using Docker Desktop, Minikube, Helm Charts, kubectl-ai, kagent as required

## Project Structure

### Documentation (this feature)

```text
specs/010-k8s-minikube-deployment/
├── plan.md              # This file (/sp.plan command output)
├── research.md          # Phase 0 output (/sp.plan command)
├── data-model.md        # Phase 1 output (/sp.plan command)
├── quickstart.md        # Phase 1 output (/sp.plan command)
├── contracts/           # Phase 1 output (/sp.plan command)
└── tasks.md             # Phase 2 output (/sp.tasks command - NOT created by /sp.plan)
```

### Source Code (repository root)
# Option 2: Web application (existing structure maintained)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
├── requirements.txt
├── Dockerfile           # NEW: Backend Dockerfile
└── .dockerignore        # NEW: Backend Docker ignore file

frontend/
├── src/
│   ├── app/
│   ├── components/
│   ├── lib/
│   └── styles/
├── package.json
├── Dockerfile           # NEW: Frontend Dockerfile
└── .dockerignore        # NEW: Frontend Docker ignore file

# Kubernetes manifests (NEW - to be created in this feature)
k8s/
├── Dockerfiles/         # Docker build files
│   ├── Dockerfile.backend
│   └── Dockerfile.frontend
├── charts/              # Helm charts
│   ├── todo-backend/
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   └── templates/
│   │       ├── deployment.yaml
│   │       ├── service.yaml
│   │       └── _helpers.tpl
│   └── todo-frontend/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── service.yaml
│           └── _helpers.tpl
├── secrets.yaml         # Kubernetes secrets definition
├── namespace.yaml       # Namespace definition
└── ingress.yaml         # Ingress configuration

# Minikube and Helm configuration
├── minikube-profile.sh  # Script to set up Minikube profile
├── helm-values.yaml     # Custom values for Helm deployments
└── k8s-setup.sh         # Automation script for full deployment
```

**Structure Decision**: Maintaining existing web application structure with frontend Next.js and backend FastAPI. Adding Dockerfiles for containerization, Helm charts for deployment, and Kubernetes manifests for cluster configuration. Creating k8s directory for all Kubernetes-related resources including Dockerfiles, Helm charts, secrets, and deployment configurations.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

Not applicable - all constitution checks passed without violations requiring justification.

## Summary of Completed Artifacts

- **research.md**: Contains all research findings and technical decisions for Kubernetes deployment
- **data-model.md**: Defines all entities for Docker images, Helm charts, and Kubernetes resources
- **quickstart.md**: Complete guide for setting up and deploying the application to Kubernetes
- **contracts/backend-openapi.yaml**: OpenAPI specification for the backend API
- **Agent Context**: Updated with new technology stack information for this feature
