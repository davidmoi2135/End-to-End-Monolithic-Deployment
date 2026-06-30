# 📘 Monolith Project Operation Guide (Professional Baseline)

This system is designed according to modern DevOps standards, focusing on stability, security, and monitoring capabilities.

## 🏗 1. System Structure
- **Backend**: Spring Boot (Java 21).
- **Frontend**: React (TypeScript).
- **Database**: PostgreSQL.
- **CI/CD**: GitHub Actions + SonarQube + Trivy.
- **Deployment**: ArgoCD + Argo Rollouts (Blue-Green Strategy).

## 🛠 2. Local Testing Guide
1. Install Docker & Docker Compose.
2. In the root directory, run:
   ```bash
   docker-compose up --build
   ```
3. Access:
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8081
   - Database: localhost:5433 (postgres/postgres)

## 🚀 3. Deployment Process (Production - GitOps)
1. **Push code**: When you push to GitHub, the pipeline will build, perform security scanning (Trivy), and quality scanning (SonarQube).
2. **Blue-Green**: Uses Argo Rollouts to deploy the new version alongside the old one, swapping traffic when the new version is stable.
3. **Monitoring**: Prometheus (Port 30090) & Grafana (Port 30300) to monitor the system.
4. **Slack**: Receive instant reports on deployment status.

## 🔑 4. Secrets Configuration
Remember to add all necessary variables to GitHub Secrets: `DOCKER_USERNAME`, `DOCKER_PASSWORD`, `SONAR_TOKEN`, `SONAR_HOST_URL`, `SLACK_WEBHOOK_URL`, `JWT_SECRET`.
