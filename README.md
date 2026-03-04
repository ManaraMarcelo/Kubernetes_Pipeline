# 🚀 CI/CD Pipeline with FastAPI, Docker, Jenkins, and Kubernetes

The main objective of this project is to teach and apply **Continuous Integration (CI)** and **Continuous Delivery (CD)** concepts in a real-world scenario. We developed a complete automation pipeline for a backend application (FastAPI) and a simple frontend, performing automated deployment to a local Kubernetes cluster via Jenkins.

Whenever new changes are pushed to the Git repository, the pipeline is automatically triggered—building, testing (future implementation), publishing, and deploying the new version of the application.

---

### 💡 Project Overview

**The Problem:** Manual software deployments are slow, error-prone, and inconsistent across environments. In modern software development, the gap between writing code and delivering it to a production-ready cluster can become a bottleneck without automation.

**The Solution:** A robust, automated "Commit-to-Cloud" pipeline. Using Jenkins as an orchestrator, the system automatically detects code changes, builds immutable Docker containers, scans them for security vulnerabilities, and deploys them into a Kubernetes cluster, ensuring that the environment always reflects the latest stable version of the code.

**Technical Challenges:**
- **Local K8s Connectivity:** Configuring Jenkins (running as a service/container) to communicate securely with a local Kubernetes cluster (Rancher Desktop/WSL) using `kubeconfig` and proper permissions.
- **Webhook Integration:** Bridging a local Jenkins instance with GitHub using `ngrok` to allow inbound Webhook events for automated triggers.
- **Security Gates:** Integrating `Trivy` into the pipeline logic to conditionally notify or block deployments based on vulnerability severity (HIGH/CRITICAL).

---

## 📌 Index
1️⃣ [Project Overview](#1️⃣-project-overview)  
2️⃣ [Technologies Used](#2️⃣-technologies-used)  
3️⃣ [Project Structure](#3️⃣-project-structure)  
4️⃣ [Phase 1: Project Preparation](#4️⃣-phase-1-project-preparation)  
5️⃣ [Phase 2: Containerization with Docker](#5️⃣-phase-2-containerization-with-docker)  
6️⃣ [Phase 3: Kubernetes Deployment Files](#6️⃣-phase-3-kubernetes-deployment-files)  
7️⃣ [Phase 4: Jenkins - Build and Push](#7️⃣-phase-4-jenkins---build-and-push)  
8️⃣ [Phase 5: Jenkins - Kubernetes Deployment](#8️⃣-phase-5-jenkins---kubernetes-deployment)  
9️⃣ [Phase 6: Documentation](#9️⃣-phase-6-documentation)  
🔟 [Extra Challenges](#🔟-extra-challenges)  
1️⃣1️⃣ [Final Application Test](#1️⃣1️⃣-final-application-test)  
✅ [Conclusion](#✅-conclusion)  

---

## 1️⃣ Project Overview
This project demonstrates the construction of a CI/CD pipeline for a Fullstack web application (FastAPI Backend + Static Frontend). The automation covers everything from code versioning on GitHub to continuous deployment in a local Kubernetes environment, using Jenkins as the orchestrator.

---

## 2️⃣ Technologies Used
* **GitHub:** Code versioning and pipeline triggering via Webhooks.
* **FastAPI:** Python web framework for Backend development.
* **Python:** Backend programming language.
* **Docker:** For containerizing the application (Backend and Frontend).
* **Docker Hub:** Public Docker image registry.
* **Jenkins:** CI/CD automation tool.
* **Local Kubernetes:** Rancher Desktop (or Minikube/Kind) for container orchestration.
* **`kubectl`:** Command-line tool for interacting with Kubernetes.
* **`trivy`:** Vulnerability scanner for Docker images (used in extra challenges).
* **`ngrok`:** To expose local Jenkins to the internet, allowing GitHub to send Webhooks.

---

## 3️⃣ Project Structure
The project is organized as follows in the Git repository:

```text
project-pipeline/  
├── src/  
│   ├── backend/  
│   │   ├── main.py  
│   │   ├── requirements.txt  
│   │   └── Dockerfile  
│   └── frontend/ # Contains HTML/CSS/JS files (served by the backend)  
│       └── index.html  
├── k8s/  
│   ├── backend-deployment.yaml 
│   └── backend-service.yaml    
└── Jenkinsfile  

```

---

## 4️⃣ Phase 1: Project Preparation

In this phase, the development environment and repositories are prepared.

### 4.1. GitHub Repository

* Created a **new repository** on GitHub (e.g., `Project-Pipeline`).
* Branch configuration:
* `dev`: Main development branch. All new features and initial tests are done here.
* `main` (or `master`): Production branch. Receives code only after complete validation in `dev`.


* **Repository Link:** `https://github.com/ManaraMarcelo/Projeto-Pipeline.git`

### 4.2. Docker Hub Account

* Created an account on Docker Hub to store the application images.
* **Docker Hub User:** `manaramarcelo`

### 4.3. Access to Local Kubernetes Cluster

* Verified Rancher Desktop (or alternative) and Kubernetes cluster access via `kubectl`.
```bash
kubectl get nodes

```



### 4.4. Local Application Validation (FastAPI Backend)

* Installed Python dependencies (`fastapi`, `uvicorn`, `httpx`, `pydantic`) and executed `main.py` locally.
```bash
cd <LOCAL_REPO_PATH>/src/backend/
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000

```


* Access in browser: `http://localhost:8000`.

---

## 5️⃣ Phase 2: Containerization with Docker

In this phase, the backend (FastAPI) is packaged into a Docker image. The frontend is bundled together with the backend in this stage.

### 5.1. Dockerfile Creation (Backend with Static Frontend)

In the `src/backend/` directory, create the `Dockerfile`.
View the full content: [Dockerfile](https://github.com/ManaraMarcelo/Projeto-Pipeline/blob/main/src/Dockerfile).

### 5.2. Local Test (Build and Execution)

Run commands to build the image and run the container locally.
Access the app at [http://localhost:8000](https://www.google.com/search?q=http://localhost:8000).

---

## 6️⃣ Phase 3: Kubernetes Deployment Files

Defined how the application will be deployed in the local Kubernetes cluster.

### 6.1. Deployment and Service YAMLs

Created the `k8s/` folder to organize Kubernetes manifests.
📁 [View Kubernetes deploy files (k8s/ folder)](https://github.com/ManaraMarcelo/Projeto-Pipeline/tree/main/k8s)

---

## 7️⃣ Phase 4: Jenkins - Build and Push

Created the Jenkins pipeline to automate the Docker image build and push.

### 7.1. New Jenkins Pipeline

1. Access Jenkins at [http://localhost:8080](https://www.google.com/search?q=http://localhost:8080).
2. Create a **"New Item"** > `fastapi-cicd-pipeline` > **"Pipeline"**.
3. Configure **SCM**: Git, **Repo URL**, **Branch**: `*/dev`, **Script Path**: `Jenkinsfile`.

🔗 [Jenkinsfile](https://github.com/ManaraMarcelo/Projeto-Pipeline/blob/main/Jenkinsfile)

### 7.3. Webhook Automation (Git Push Trigger)

#### On Jenkins:

* Enable **"GitHub hook trigger for GITScm polling"**.

#### On GitHub:

1. Go to **Settings** > **Webhooks** > **"Add webhook"**.
2. **Payload URL**: `https://<YOUR_NGROK_URL>/github-webhook/`
3. **Content type**: `application/json`

---

## 8️⃣ Phase 5: Jenkins - Kubernetes Deployment

The pipeline is extended to deploy the application directly to Kubernetes.

### 8.1. Jenkins Access to `kubectl`

* ✅ `kubectl` must be in the Jenkins user `PATH`.
* ✅ `kubeconfig` from Rancher Desktop copied to `/var/lib/jenkins/.kube/config` with correct permissions.
* ✅ Jenkins Credential (Secret Text) created with the `kubeconfig` content (ID: `kubeconfig`).

---

## 9️⃣ Phase 6: Documentation

**Deliverables:** Complete `README.md` with all steps.

---

## 🔟 Extra Challenges

### 10.1. Vulnerability Scanning with Trivy

Installed Trivy and jq on the Jenkins agent. Added a "Vulnerability Scan" stage in the Jenkinsfile.

### 10.2. Slack Notifications

Implemented automated notifications to inform the team about build status and security scan results.

---

## 1️⃣1️⃣ Final Application Test

Access the application in the browser via:

`http://localhost:<NodePort_allocated_by_K8s>` (Example: [http://localhost:30001](https://www.google.com/search?q=http://localhost:30001))

### Verifications:

* ✅ Check **FastAPI** endpoints (`/color`, `/cat`, etc.).
* ✅ Confirm **frontend** is being served correctly.
* ✅ Check **Slack notifications**.

---

## ✅ Conclusion

This project demonstrates a **robust and automated CI/CD pipeline** for containerized applications, covering everything from development to **Kubernetes** deployment while integrating essential **DevOps** tools.

The automated workflow ensures:

* 🔁 **Faster deliveries**
* ✅ **Reliability**
* 📈 **Higher quality**
* 🛡️ **Minimized manual errors**

---

## 🔗 Contact

📧 **Email:** [zmarcelo2018@gmail.com](mailto:zmarcelo2018@gmail.com)

💡 *Suggestions and improvements are always welcome!*

```
