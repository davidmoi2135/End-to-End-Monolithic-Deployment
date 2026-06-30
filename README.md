# ECOMMERCE MONOLITHIC DEPLOYMENT PIPELINE

A Full-stack e-commerce system designed to illustrate a modern DevSecOps deployment pipeline on a Kubernetes platform.

---

## SYSTEM ARCHITECTURE

<img width="986" height="515" alt="image" src="https://github.com/user-attachments/assets/a31533b5-7967-4097-a18b-12daecded966" />

The project focuses on automating the software development lifecycle, including these components:
- User Interface: React (TypeScript)
- Business Logic: Spring Boot (Java 21)
- Data Storage: PostgreSQL
- Deployment Infrastructure: Kubernetes Cluster

---

## TECHNOLOGIES USED

BACKEND
- Java 21 and Spring Boot 4.x
- Spring Security with JWT authentication mechanism
- Spring Data JPA connecting to PostgreSQL

FRONTEND
- React 18 and TypeScript
- Axios for API communication

DEVOPS AND INFRASTRUCTURE
- Infrastructure as Code: Terraform
- Configuration Management: Ansible (Kubespray)
- CI/CD Pipeline: GitHub Actions
- Container Management: Docker
- Container Orchestration: Kubernetes Cluster (self-managed)
- GitOps: ArgoCD and Argo Rollouts
- Monitoring: Prometheus and Grafana
- Security: Image vulnerability scanning with Trivy

---

## INFRASTRUCTURE DEPLOYMENT GUIDE

1. RESOURCE INITIALIZATION (TERRAFORM)
- Navigate to the terraform directory: `cd terraform`
- Initialize and execute:
  ```bash
  terraform init
  terraform apply -auto-approve
  ```
- Result: Terraform will create EC2 Instances on AWS and automatically generate the inventory file at `inventory/mycluster/hosts.yaml`.

2. KUBERNETES CLUSTER INSTALLATION (KUBESPRAY)
- Ensure Ansible is installed and Kubespray is cloned.
- Run the playbook to install the cluster:
  ```bash
  ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root cluster.yml
  ```

3. ARGOCD INSTALLATION
- Create namespace and install:
  ```bash
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```
- Access the Dashboard:
  ```bash
  kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
  ```

4. MONITORING INSTALLATION (PROMETHEUS & GRAFANA)
- Use Helm to install the Kube-Prometheus-Stack:
  ```bash
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  helm repo update
  helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
  ```

---

## CI/CD PROCESS

1. CODE INTEGRATION
- Automatically tests and builds source code upon changes on the main branch.
- Performs security scanning of source code and Docker images using Trivy to detect vulnerabilities.

2. ARTIFACT MANAGEMENT
- Packages the application into Docker images.
- Automatically tags and pushes images to Docker Hub.

3. GITOPS WORKFLOW
- Pipeline automatically updates the Manifest YAML (Image Tag) in the configuration repository.
- ArgoCD monitors the repository and automatically synchronizes (syncs) the desired state to the Kubernetes cluster.

4. BLUE-GREEN DEPLOYMENT
- Uses Argo Rollouts to perform Blue-Green deployment.
- Switches traffic between the Active Service and Preview Service to ensure Zero Downtime and enable instant Rollback.

---

## LOCAL RUN GUIDE (TESTING)

1. Clone repository:
   git clone https://github.com/davidmoi2135/End-to-End-Monolithic-Deployment.git

2. Launch with Docker Compose:
   docker-compose up --build

3. Access the application:
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8081
