# Full-Cycle CI/CD Pipeline: Automated FastAPI Deployment with Jenkins & Kubernetes

The main objective of this project is to apply **Continuous Integration (CI)** and **Continuous Delivery (CD)** concepts in a real-world architectural scenario. We developed a complete automation pipeline for a FastAPI backend and a static frontend, performing automated deployments to a local Kubernetes cluster via Jenkins.

---

### 💡 Project Overview

**The Problem:** Manual software deployments are slow, inconsistent, and highly prone to human error. In a modern development cycle, the manual transition from code to a production-ready cluster creates bottlenecks and security risks.

**The Solution:** A robust, "Commit-to-Cloud" automated pipeline. Using Jenkins as the central orchestrator, the system automatically detects code changes via Webhooks, builds immutable Docker images, performs security scans with Trivy, and deploys the application into a Kubernetes cluster.



**Technical Challenges:**
- **Local K8s Connectivity:** Configuring Jenkins (running as a service) to securely communicate with a local Kubernetes cluster (Rancher Desktop/WSL) using `kubeconfig` and specific RBAC permissions.
- **Webhook Integration:** Using `ngrok` to bridge a local Jenkins instance with GitHub, enabling inbound event triggers for a seamless automation flow.
- **Security Gates:** Implementing `Trivy` as a mandatory step to audit container images, allowing the pipeline to fail or notify based on vulnerability severity (HIGH/CRITICAL).

---

## 📌 Index
1. [Technologies Used](#-technologies-used)
2. [Project Structure](#-project-structure)
3. [Phase 1: Project Preparation](#-phase-1-project-preparation)
4. [Phase 2: Containerization (Docker)](#-phase-2-containerization-docker)
5. [Phase 3: Kubernetes Manifests](#-phase-3-kubernetes-manifests)
6. [Phase 4: Jenkins Automation (Build & Push)](#-phase-4-jenkins-automation-build--push)
7. [Phase 5: Jenkins Deployment (K8s)](#-phase-5-jenkins-deployment-k8s)
8. [Phase 6: Extra Challenges (Security & Alerts)](#-phase-6-extra-challenges-security--alerts)
9. [Final Testing & Validation](#-final-testing--validation)
10. [Conclusion](#-conclusion)
11. [Contact](#-contact)

---

## 🛠️ Technologies Used
* **GitHub:** Version control and Webhook triggers.
* **FastAPI:** Python framework for backend development.
* **Docker & Docker Hub:** Containerization and image registry.
* **Jenkins:** CI/CD Automation orchestrator.
* **Kubernetes (K8s):** Rancher Desktop for container orchestration.
* **Trivy:** Security scanner for vulnerability detection.
* **Ngrok:** Local tunnel for GitHub Webhook integration.

---

## 📁 Project Structure

```text
fullcycle-fastapi-jenkins-k8s/  
├── src/  
│   ├── backend/  
│   │   ├── main.py  
│   │   ├── requirements.txt  
│   │   └── Dockerfile  
│   └── frontend/ # Static assets served by backend  
│       └── index.html  
├── k8s/  
│   ├── backend-deployment.yaml 
│   └── backend-service.yaml    
└── Jenkinsfile  

```

---

## 🚀 Phase 1: Project Preparation

* **GitHub:** Repository created with a `dev` branch for staging and `main` for production.
* **Docker Hub:** Account configured for image storage (`manaramarcelo`).
* **Kubernetes:** Cluster access verified via `kubectl get nodes`.

## 🐳 Phase 2: Containerization (Docker)

The backend and frontend are bundled into a single optimized Docker image.

* **Build & Test:** Verified locally at `http://localhost:8000`.
* 🔗 **Full Content:** [Dockerfile](https://www.google.com/search?q=./src/backend/Dockerfile)

## ☸️ Phase 3: Kubernetes Manifests

Standardized deployment files were created to manage replicas and services.

* 📁 [View k8s manifests](https://www.google.com/search?q=./k8s)

## 🏗️ Phase 4: Jenkins Automation (Build & Push)

Configured a Pipeline script from SCM pointing to the `Jenkinsfile`.

* **Trigger:** Configured **GitHub hook trigger for GITScm polling** via Ngrok.
* 🔗 [Jenkinsfile](https://www.google.com/search?q=./Jenkinsfile)

## 🚢 Phase 5: Jenkins Deployment (K8s)

Extended the pipeline to include the deployment stage.

* **Access:** Jenkins was granted `kubeconfig` access via "Secret Text" credentials.
* **Process:** Automated `kubectl apply` with dynamic image tagging based on `BUILD_ID`.

## 🛡️ Phase 6: Extra Challenges (Security & Alerts)

* **Trivy Scan:** Automated stage to detect HIGH and CRITICAL vulnerabilities.
* **Slack Notifications:** Real-time feedback on pipeline success or security failures.

---

## ✅ Final Testing & Validation

Access via `http://localhost:30001` (NodePort).

* **Backend:** Verified endpoints `/color` and `/cat`.
* **Frontend:** Verified UI rendering and interaction.
* **Alerts:** Verified Slack messages for build status.

## 🏁 Conclusion

This project establishes a **robust, automated CI/CD pipeline**, minimizing manual intervention and maximizing security and delivery speed. It bridges the gap between development and production-grade orchestration.

---

## 🔗 Contact

**Marcelo Manara** [LinkedIn](https://www.linkedin.com/in/marcelo-manara) | [Portfolio](https://portifolio-peach-beta.vercel.app/)
