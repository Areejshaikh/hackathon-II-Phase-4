---

description: "Task list for Phase IV: Local Kubernetes Deployment"
---

# Tasks: Phase IV: Local Kubernetes Deployment

**Input**: Design documents from `/specs/010-k8s-minikube-deployment/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: The feature specification does not explicitly request test tasks, so they will not be included.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Web app**: `backend/src/`, `frontend/src/`
- Paths shown below assume web app structure - adjust based on plan.md structure

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [x] T001 Verify prerequisites: Docker Desktop, Minikube, kubectl, Helm installed
- [x] T002 [P] Create /k8s directory structure for Kubernetes manifests
- [x] T003 [P] Create /k8s/Dockerfiles directory for Docker build files
- [x] T004 [P] Create /k8s/charts directory structure for Helm charts

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T005 Create Dockerfile for backend (FastAPI) with multi-stage build
- [x] T006 Create Dockerfile for frontend (Next.js) with multi-stage build
- [x] T007 [P] Create .dockerignore files for both frontend and backend
- [x] T008 Create Helm chart directory structure for todo-backend
- [x] T009 Create Helm chart directory structure for todo-frontend
- [x] T010 Create initial Chart.yaml files for both Helm charts
- [x] T011 [P] Create values.yaml files for both Helm charts with default configurations
- [x] T012 Create namespace.yaml for todo-chatbot namespace
- [x] T013 Create secrets.yaml template for Kubernetes secrets

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Local Kubernetes Deployment (Priority: P1) 🎯 MVP

**Goal**: Deploy the existing Todo Chatbot application to a local Kubernetes cluster (Minikube) so that it becomes cloud-native, zero-cost, and beginner-friendly with AI-assisted tools.

**Independent Test**: The application can be successfully deployed to a local Minikube cluster with all services running and accessible, and all Phase III features working identically.

### Implementation for User Story 1

- [ ] T014 [US1] Create backend deployment.yaml template in Helm chart
- [ ] T015 [US1] Create backend service.yaml template in Helm chart
- [ ] T016 [US1] Create frontend deployment.yaml template in Helm chart
- [ ] T017 [US1] Create frontend service.yaml template in Helm chart
- [ ] T018 [US1] Configure ingress.yaml for external access to frontend
- [ ] T019 [US1] Set up Minikube with Docker driver and appropriate resources (4096MB, 2 CPUs)
- [ ] T020 [US1] Build backend Docker image: docker build -t todo-backend:latest .
- [ ] T021 [US1] Build frontend Docker image: docker build -t todo-frontend:latest .
- [ ] T022 [US1] Load Docker images into Minikube: minikube image load todo-backend:latest
- [ ] T023 [US1] Load frontend image into Minikube: minikube image load todo-frontend:latest
- [ ] T024 [US1] Create Kubernetes secrets for environment variables
- [ ] T025 [US1] Install backend Helm chart: helm install todo-backend ./k8s/charts/todo-backend
- [ ] T026 [US1] Install frontend Helm chart: helm install todo-frontend ./k8s/charts/todo-frontend
- [ ] T027 [US1] Verify pods are running: kubectl get pods --namespace=todo-chatbot
- [ ] T028 [US1] Verify services are accessible: kubectl get services --namespace=todo-chatbot
- [ ] T029 [US1] Test application accessibility via Minikube service URL
- [ ] T030 [US1] Verify all Phase III features (login, tasks, chat) work identically

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - Containerization & Helm Charts (Priority: P2)

**Goal**: Containerize the frontend and backend applications and create Helm charts so that the deployment process is standardized, repeatable, and manageable through Kubernetes.

**Independent Test**: Docker images are created for both frontend and backend, and Helm charts are generated that can successfully deploy the application to Kubernetes.

### Implementation for User Story 2

- [ ] T031 [US2] Optimize backend Dockerfile with python:3.11-slim base image
- [ ] T032 [US2] Optimize frontend Dockerfile with multi-stage build for production
- [ ] T033 [US2] Add health checks to both Dockerfiles
- [ ] T034 [US2] Create _helpers.tpl template files for both Helm charts
- [ ] T035 [US2] Configure environment variables in Helm charts for frontend
- [ ] T036 [US2] Configure environment variables in Helm charts for backend
- [ ] T037 [US2] Add resource limits and requests to Helm chart deployments
- [ ] T038 [US2] Add replica count configuration to Helm charts
- [ ] T039 [US2] Create ConfigMap templates in Helm charts if needed
- [ ] T040 [US2] Test Helm chart upgrades and rollbacks
- [ ] T041 [US2] Verify Docker images are under 500MB after optimization
- [ ] T042 [US2] Document Helm chart customization options

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - AI-Assisted Operations (Priority: P3)

**Goal**: Use AI-assisted tools (Gordon, kubectl-ai, kagent) for Docker and Kubernetes operations so that deployment and management tasks are simplified and more efficient.

**Independent Test**: AI tools can successfully assist with Docker builds, Kubernetes deployments, scaling, and debugging operations.

### Implementation for User Story 3

- [ ] T043 [US3] Test Gordon (Docker AI) for Dockerfile generation and optimization
- [ ] T044 [US3] Create Docker build automation using Gordon if available
- [ ] T045 [US3] Test kubectl-ai for deployment operations
- [ ] T046 [US3] Test kubectl-ai for scaling operations: "scale todo-backend to 3 replicas"
- [ ] T047 [US3] Test kubectl-ai for debugging operations: "check why pods are failing"
- [ ] T048 [US3] Test kagent for cluster health analysis
- [ ] T049 [US3] Document AI-assisted commands for common operations
- [ ] T050 [US3] Implement fallback mechanisms when AI tools are unavailable
- [ ] T051 [US3] Verify AI tools successfully perform at least 80% of operations
- [ ] T052 [US3] Create automation scripts that incorporate AI tools

**Checkpoint**: All user stories should now be independently functional

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T053 [P] Update documentation with final Docker build and run commands
- [ ] T054 [P] Create k8s-setup.sh automation script for full deployment
- [ ] T055 [P] Create minikube-profile.sh script for consistent Minikube setup
- [ ] T056 [P] Add proper error handling and logging to automation scripts
- [ ] T057 [P] Optimize resource usage to meet success criteria (under 500MB per container)
- [ ] T058 [P] Ensure deployment completes within 5 minutes as per success criteria
- [ ] T059 [P] Verify 99% uptime during normal operation in Kubernetes environment
- [ ] T060 Run complete validation using quickstart guide procedures

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - Builds upon US1 with Helm chart enhancements
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - Can work alongside US1/US2 for AI tool integration

### Within Each User Story

- Models before services
- Services before endpoints
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Launch all Docker builds for User Story 1 together:
docker build -t todo-backend:latest .  # Backend image build
docker build -t todo-frontend:latest . # Frontend image build

# Launch all image loading to Minikube together:
minikube image load todo-backend:latest  # Load backend image
minikube image load todo-frontend:latest  # Load frontend image

# Launch all Helm installations together:
helm install todo-backend ./k8s/charts/todo-backend --namespace=todo-chatbot --create-namespace  # Backend install
helm install todo-frontend ./k8s/charts/todo-frontend --namespace=todo-chatbot  # Frontend install
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence