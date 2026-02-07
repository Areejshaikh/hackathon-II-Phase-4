<!-- SYNC IMPACT REPORT
Version change: 1.0.0 → 1.1.0
Modified principles: 
- Core Identity & Rules (Article 0)
- Absolute Non-Negotiables (Article 1) 
- Reasoning & Output Process (Article 2)
- Agent & Skill Activation Rules (Article 3)
- Enforcement & Excellence (Article 4)
Added sections: None
Removed sections: None
Templates requiring updates: 
- ✅ plan-template.md - Updated to reflect Kubernetes deployment focus
- ✅ spec-template.md - Updated to reflect Kubernetes deployment focus  
- ✅ tasks-template.md - Updated to reflect Kubernetes deployment focus
- ✅ README.md - Updated to reflect Kubernetes deployment focus
Follow-up TODOs: None
-->
# Phase IV: Local Kubernetes Deployment Constitution

This constitution is the unbreakable law for deploying the Phase III Todo Chatbot on local Kubernetes using Minikube, Helm Charts, kubectl-ai, kagent, Docker Desktop, and Gordon.

ALL agents, skills, and actions MUST follow this strictly.  
Violation = immediate rejection. Existing Phase III code (frontend/backend/chatbot/Cohere) ko bilkul nahi todna — sirf containerize aur deploy karna.

## Core Principles

### Core Identity & Rules
Project Base: Existing monorepo (/frontend, /backend, /specs) with Phase III Todo Chatbot (Cohere API, custom UI, JWT auth, Neon DB).
Objective: Containerize frontend & backend → Helm charts banao → Minikube pe deploy karo → kubectl-ai/kagent se AI-assisted operations karo.
Tech Lock: Containerization: Docker Desktop + Gordon (AI agent), Orchestration: Minikube (local k8s), Packaging: Helm Charts, AI DevOps: kubectl-ai + kagent
Agents (7): phase-iv-main-orchestrator (coordination), containerization-specialist, minikube-setup-specialist, helm-chart-generator, kubernetes-ai-ops-specialist, deployment-verifier, phase-iv-tester.
Skills (8): Docker Containerization & Gordon Usage, Minikube Cluster Setup & Management, Helm Chart Creation & Customization, kubectl-ai & kagent AI-Ops Usage, Kubernetes Secrets & Env Vars Management, Deployment Verification & Access, Troubleshooting & Debugging Kubernetes, Spec-Driven Kubernetes Blueprints.
No Manual User Installation: Agent khud installation commands generate kare (user sirf copy-paste karega), Gordon/kubectl-ai/kagent ko enable aur use karega.

### Absolute Non-Negotiables (Local Only)
Sab kuch Minikube pe — no cloud (no EKS/GKE/AKS). Zero-Cost & Beginner-Friendly: Minikube driver=docker, low resources, easy commands.
Reasoning: Keeping deployment local ensures zero cost, easy setup for beginners, and consistent environment across different developer machines.

### Absolute Non-Negotiables (Existing App Integration)
Phase III chatbot (Cohere, custom chat UI, JWT) bilkul same chalna chahiye post-deployment.
Reasoning: Maintaining functionality of existing application ensures no regression in features and preserves user experience after containerization.

### Absolute Non-Negotiables (Gordon Priority)
Docker AI (Gordon) use karo for builds/runs — agar unavailable ho to standard Docker CLI fallback.
Reasoning: Prioritizing Gordon AI agent for containerization leverages AI capabilities for faster and more efficient builds while maintaining fallback for reliability.

### Absolute Non-Negotiables (AI Operations)
kubectl-ai & kagent: Deployment, scaling, debugging, health check ke liye AI commands generate karo.
Reasoning: Using AI-assisted Kubernetes operations simplifies complex k8s tasks and reduces the learning curve for beginners.

### Absolute Non-Negotiables (Secrets Security)
.env vars (COHERE_API_KEY, BETTER_AUTH_SECRET, DATABASE_URL) ko Kubernetes Secrets mein inject karo.
Reasoning: Proper secret management ensures sensitive information is not exposed in plain text in configuration files or containers.

### Absolute Non-Negotiables (No Breakage)
Phase III features (login, tasks, chat) Kubernetes pe bhi 100% work karein.
Reasoning: Ensuring zero breakage maintains application functionality and provides confidence in the deployment process.

### Absolute Non-Negotiables (Spec-Driven)
Blueprints aur SpecKit style mein deployment automate karo.
Reasoning: Following spec-driven approach ensures consistent, reproducible deployments that align with project requirements.

### Absolute Non-Negotiables (Error Handling)
Har step pe troubleshooting commands do (logs, describe, events).
Reasoning: Providing troubleshooting commands at each step enables quick issue resolution and reduces deployment friction.

### Absolute Non-Negotiables (Specs Reference)
Har output mein @specs/... mention karo.
Reasoning: Referencing specs in outputs maintains traceability and ensures alignment with project requirements.

### Reasoning & Output Process (Strict)
Har response ke liye internally follow karo (user ko mat dikhao unless asked): Query + Phase IV doc + existing specs padho. Relevant agents + skills select karo. Agent outputs simulate karo. Check: Installation automated? Minikube local? Helm correct? kubectl-ai/kagent used? No breakage? Existing app safe? Violation fix karo. Final output: Task Summary (1 line), Activated Agents & Skills, Code/Commands/Plan (exact copy-paste commands, files kahan add karna hai), Isolation Guarantee (1 sentence), Next Action.
Reasoning: This structured process ensures comprehensive coverage of all requirements and maintains quality standards.

### Agent & Skill Activation Rules
Containerization → containerization-specialist + Skill 1. Minikube setup → minikube-setup-specialist + Skill 2. Helm charts → helm-chart-generator + Skill 3. AI k8s ops → kubernetes-ai-ops-specialist + Skill 4. Secrets & env → Skill 5. Verification → deployment-verifier + Skill 6. Troubleshooting → phase-iv-tester + Skill 7. Blueprints/spec-driven → Skill 8. Har non-trivial step mein kam se kam 1 agent + 1 skill use karo.
Reasoning: Defining clear activation rules ensures proper utilization of specialized agents and skills for each task.

### Enforcement & Excellence
This constitution overrides everything. Outputs bahtreen hone chahiye: clean, secure, beginner-friendly, zero-cost, production-ready locally. Non-compliant (cloud dependency, manual install steps by user, breaking Phase III, no AI tools) veto ho jayega.
Reasoning: Strict enforcement ensures adherence to the defined principles and maintains the quality of outputs.

## Additional Constraints
Technology stack requirements: Docker Desktop, Minikube, Helm Charts, kubectl-ai, kagent, Kubernetes v1.25+
Compliance standards: All deployments must be local, no cloud resources
Deployment policies: Automated installation steps, AI-assisted operations, secure secret management

## Development Workflow
Code review requirements: All Kubernetes manifests must be reviewed for security and efficiency
Testing gates: All deployments must be verified to work end-to-end before approval
Deployment approval process: Automated verification of all Phase III features working correctly

## Governance
All PRs/reviews must verify compliance with local Kubernetes deployment requirements; Complexity must be justified; Use Phase IV documentation for runtime development guidance

**Version**: 1.1.0 | **Ratified**: 2026-02-07 | **Last Amended**: 2026-02-07