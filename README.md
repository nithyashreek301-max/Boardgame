# Enterprise-Grade DevSecOps CI/CD Pipeline on Kubernetes

![Pipeline Status](https://img.shields.io/badge/Pipeline-Passing-brightgreen)
![Security Scan](https://img.shields.io/badge/Security-Trivy-blue)
![Kubernetes](https://img.shields.io/badge/Orchestration-Kubernetes-326CE5)
![AWS](https://img.shields.io/badge/Cloud-AWS-FF9900)

---

## About the Project

Built a production-grade end-to-end CI/CD pipeline that automates the entire software delivery lifecycle — from code commit to Kubernetes deployment — with integrated security scanning, quality gates, artifact management, and real-time monitoring.

---

## Architecture Overview

```
Developer Pushes Code
        ↓
    GitHub Repo
        ↓
    Jenkins (CI/CD Orchestrator)
        ↓
┌─────────────────────────────────┐
│         Pipeline Stages          │
│  Compile → Test → Scan →        │
│  Quality Gate → Build →         │
│  Dockerize → Push → Deploy      │
└─────────────────────────────────┘
        ↓
  Kubernetes Cluster
        ↓
  Deployed Application
        ↓
  Prometheus + Grafana (Monitoring)
        ↓
  Email Notification (Success/Failure)
```

---

## Tools & Technologies

| Category | Tools |
|---|---|
| Version Control | GitHub |
| CI/CD | Jenkins, Maven |
| Code Quality | SonarQube |
| Security Scanning | Trivy |
| Artifact Management | Nexus Repository |
| Containerization | Docker |
| Container Orchestration | Kubernetes |
| Monitoring | Prometheus, Grafana |
| Notification | Email (SMTP) |
| Cloud | AWS EC2 |
| OS | Ubuntu Linux |

---

## Pipeline Stages

| Stage | Tool | Purpose |
|---|---|---|
| 1. Tool Install | Jenkins | Install JDK17 and Maven |
| 2. Git Checkout | GitHub | Pull latest source code |
| 3. Compile | Maven | Compile source code |
| 4. Test | Maven | Run unit test cases |
| 5. File System Scan | Trivy | Scan filesystem for vulnerabilities |
| 6. SonarQube Analysis | SonarQube | Code quality and coverage analysis |
| 7. Quality Gate | SonarQube | Fail pipeline if quality standards not met |
| 8. Build | Maven | Package application as JAR |
| 9. Publish to Nexus | Nexus | Store versioned build artifact |
| 10. Build Docker Image | Docker | Containerize the application |
| 11. Image Scan | Trivy | Scan Docker image for vulnerabilities |
| 12. Push Docker Image | Docker Hub | Push image to container registry |
| 13. Deploy to Kubernetes | kubectl | Deploy containerized app to K8s cluster |
| 14. Verify Deployment | kubectl | Confirm pods and services are running |
| 15. Send Email | SMTP | Notify team with build status + Trivy report |

---

## Infrastructure Setup

### Local DevOps Environment
- Jenkins configured locally for CI/CD pipeline orchestration
- SonarQube deployed as a Docker container for static code quality analysis
- Nexus Repository Manager deployed as a Docker container for artifact storage and version management

### Kubernetes Cluster
- Local Kubernetes cluster created using Kind (Kubernetes in Docker)
- Used for container orchestration and application deployment testing
- Networking handled through default Kind networking with Kubernetes services
- Ingress configured using NGINX Ingress Controller for external application access

---

## Screenshots

### Jenkins Pipeline — All Stages Passing ✅
![Jenkins Pipeline](https://raw.githubusercontent.com/nithyashreek301-max/Boardgame/master/screenshots/Jenkins_pipeline.png)

### Jenkins Build Status — Build #29 Success ✅
![Jenkins Build](https://raw.githubusercontent.com/nithyashreek301-max/Boardgame/master/screenshots/jenkins_build.png)

### SonarQube Code Quality Analysis ✅
![SonarQube](https://raw.githubusercontent.com/nithyashreek301-max/Boardgame/master/screenshots/Sonar_Qube.png)

### Kubernetes Pods Running ✅
![K8s Pods](https://raw.githubusercontent.com/nithyashreek301-max/Boardgame/master/screenshots/Kubernetes_pod.png)

### Email Notification with Trivy Report Attached ✅
![Email Notification](https://raw.githubusercontent.com/nithyashreek301-max/Boardgame/master/screenshots/Mail_notification.png)

### Trivy Security Scan Report ✅
> Full Trivy scan report is attached via email notification 
> after each pipeline run. See Email Notification screenshot above.
---

## Security Scanning Results

### Docker Image Scan Summary
| Target | Type | Total | Critical | High | Medium | Low |
|---|---|---|---|---|---|---|
| Alpine 3.23.3 (base image) | OS | 10 | 0 | 5 | 5 | 0 |
| app.jar (Java dependencies) | JAR | 91 | 13 | 38 | 32 | 8 |

> **Note:** Vulnerabilities detected are in open-source dependencies of the application. In a production environment, these would be flagged to the development team to upgrade affected libraries to their fixed versions. All vulnerabilities have available fixes as identified by Trivy.

---

## Key Highlights

- ✅ **Multiple successful builds** — Pipeline ran 29+ times with consistent results
- ✅ **Real production scale** — Deployed and verified on live Kubernetes cluster
- ✅ **DevSecOps integrated** — Security scanning at both filesystem and image level
- ✅ **Full automation** — Zero manual intervention from commit to deployment
- ✅ **Observability** — Monitoring via Prometheus and Grafana
- ✅ **Notifications** — Automated email with build status and security reports

---

## How to Run

### Prerequisites
- Local machine or VM with Docker installed
- Kind (Kubernetes in Docker) installed
- kubectl configured for cluster access
- Sufficient system resources (minimum 8 GB RAM recommended)
- Internet connectivity for pulling Docker images and dependencies

### Steps
```bash
# 1. Clone the repository
git clone https://github.com/nithyashreek301-max/Boardgame.git

# 2. Set up Jenkins and install required plugins
# (Eclipse Temurin, Pipeline Maven, SonarQube Scanner,
#  Docker, Kubernetes CLI, Config File Provider)

# 3. Configure credentials in Jenkins
# - GitHub token
# - SonarQube token
# - Docker Hub credentials
# - Kubernetes config

# 4. Create and run the pipeline
# - Create new Jenkins Pipeline job
# - Point to Jenkinsfile in repo
# - Build Now

# 5. Verify deployment
kubectl get pods -n webapps
kubectl get svc -n webapps
```

---

## Key Learnings

- Understood how **quality gates** prevent bad code from reaching production
- Learned how **Trivy** catches vulnerabilities at both filesystem and container image level
- Implemented **end-to-end automation** reducing manual deployment effort to zero
- Configured **Prometheus and Grafana** for real-time application monitoring
- Understood **artifact versioning** in Nexus to prevent deployment of untested builds
- Gained hands-on experience with **Kubernetes** pod management and service exposure

---

## Author

**Nithyashree K**
- LinkedIn: [linkedin.com/in/nithya30](https://linkedin.com/in/nithya30)
- GitHub: [github.com/nithyashreek301-max](https://github.com/nithyashreek301-max)
