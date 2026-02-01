# GitOps-Driven Cloud-Native Deployment Pipeline

![Go](https://img.shields.io/badge/go-%2300ADD8.svg?style=for-the-badge&logo=go&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![ArgoCD](https://img.shields.io/badge/argo-EF7B4D.svg?style=for-the-badge&logo=argo&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Helm](https://img.shields.io/badge/helm-%230F1689.svg?style=for-the-badge&logo=helm&logoColor=white)

**Enterprise-grade automated deployment infrastructure implementing GitOps principles with declarative Kubernetes configuration, multi-stage container optimization, and continuous delivery automation on AWS cloud.**

## 🎯 Project Purpose

This project demonstrates production-ready DevOps engineering practices through a fully automated CI/CD pipeline that showcases:

- **Zero-touch deployments** from code commit to production
- **Security-first approach** with distroless containers and secrets management  
- **Infrastructure as Code** using Helm charts and Kubernetes manifests
- **GitOps methodology** with ArgoCD for declarative continuous delivery
- **Cloud-native architecture** on AWS EKS with automated scaling capabilities

**Ideal for demonstrating:** DevOps Engineer, Cloud Engineer, SRE, Platform Engineer, or Kubernetes Administrator role competencies.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Local Development](#local-development)
- [CI/CD Pipeline](#cicd-pipeline)
- [Kubernetes Deployment](#kubernetes-deployment)
- [GitOps with ArgoCD](#gitops-with-argocd)
- [Testing the Pipeline](#testing-the-pipeline)
- [Cleanup](#cleanup)
- [Skills Demonstrated](#skills-demonstrated)
- [Troubleshooting](#troubleshooting)

## 🎯 Overview

This project showcases a complete DevOps workflow for deploying a Go web application with:

- **Multi-stage Docker builds** for optimized container images using distroless base
- **GitHub Actions CI/CD** with build, test, code quality, and automated deployment
- **Helm charts** for environment-specific Kubernetes configurations
- **ArgoCD** for GitOps continuous delivery
- **AWS EKS** as the production Kubernetes cluster

### Key Features

| Feature | Technology |
|---------|-----------|
| **Application** | Go 1.22.5 web server |
| **Containerization** | Multi-stage Docker build with distroless |
| **CI Pipeline** | GitHub Actions (build, test, lint, push) |
| **CD Pipeline** | ArgoCD GitOps |
| **Package Management** | Helm 3 |
| **Orchestration** | Kubernetes on AWS EKS |
| **Ingress** | NGINX Ingress Controller |

## 🏗️ Architecture

### CI/CD Flow

```
┌─────────────────┐
│   Developer     │
│   Git Push      │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────────────────────┐
│         GitHub Actions (CI)                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐ │
│  │  Build   │→ │   Test   │→ │   Lint   │ │
│  └──────────┘  └──────────┘  └──────────┘ │
│         │                                   │
│         ▼                                   │
│  ┌──────────────────────────────────────┐ │
│  │  Docker Build & Push to DockerHub   │ │
│  └──────────────────────────────────────┘ │
│         │                                   │
│         ▼                                   │
│  ┌──────────────────────────────────────┐ │
│  │  Update Helm values.yaml with tag   │ │
│  └──────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────┐
│            ArgoCD (CD)                      │
│  • Monitors Git repository                  │
│  • Detects values.yaml changes             │
│  • Syncs to EKS cluster                     │
└─────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────┐
│          AWS EKS Cluster                    │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐    │
│  │  Deploy │→ │ Service │→ │ Ingress │    │
│  └─────────┘  └─────────┘  └─────────┘    │
└─────────────────────────────────────────────┘
         │
         ▼
    Application Live!
```

## 📁 Project Structure

```
go-web-app/
├── .github/
│   └── workflows/
│       └── ci.yaml              # GitHub Actions CI pipeline
├── helm/
│   └── go-web-app-chart/
│       ├── templates/
│       │   ├── deployment.yaml  # K8s Deployment manifest
│       │   ├── service.yaml     # K8s Service manifest
│       │   └── ingress.yaml     # K8s Ingress manifest
│       └── values.yaml          # Helm configuration values
├── k8s/
│   └── manifests/
│       ├── deployment.yaml      # Standalone K8s manifests
│       ├── service.yaml
│       └── ingress.yaml
├── static/
│   └── home.html                # Static web content
├── Dockerfile                   # Multi-stage Docker build
├── go.mod                       # Go module dependencies
├── go.sum                       # Go dependency checksums
├── main.go                      # Application entry point
└── README.md
```

## ✅ Prerequisites

### Required Tools

- [Go](https://golang.org/dl/) (v1.22+)
- [Docker](https://docs.docker.com/get-docker/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [AWS CLI](https://aws.amazon.com/cli/)
- [eksctl](https://eksctl.io/)
- [Helm](https://helm.sh/docs/intro/install/) (v3+)

### Required Accounts

- AWS account with EKS permissions
- Docker Hub account
- GitHub account

### Verify Installation

```bash
go version        # Should show 1.22+
docker --version
kubectl version --client
aws --version
eksctl version
helm version
```

## 🚀 Local Development

### Step 1: Clone and Initialize

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/go-web-app.git
cd go-web-app

# Initialize Go module
go mod init github.com/YOUR_USERNAME/go-web-app
go mod tidy
```

### Step 2: Build and Test Locally

```bash
# Build the application
go build -o go-web-app

# Run tests
go test ./...

# Run the application
./go-web-app
```

Visit `http://localhost:8080` to view the application.

### Step 3: Build Docker Image

```bash
# Build multi-stage Docker image
docker build -t tobeablessing/go-web-app:v1 .

# Run container locally
docker run -p 8080:8080 -it tobeablessing/go-web-app:v1

# Push to Docker Hub (after docker login)
docker push tobeablessing/go-web-app:v1
```

## 🔄 CI/CD Pipeline

### GitHub Actions Workflow

The CI pipeline (`.github/workflows/ci.yaml`) performs the following stages:

#### 1. **Build Stage**
- Checks out repository
- Sets up Go 1.22
- Builds the application
- Runs unit tests

#### 2. **Code Quality Stage**
- Runs `golangci-lint` for static code analysis
- Ensures code meets quality standards

#### 3. **Push Stage**
- Builds multi-stage Docker image
- Tags image with GitHub run ID
- Pushes to Docker Hub

#### 4. **Helm Update Stage**
- Updates `values.yaml` with new image tag
- Commits and pushes changes back to repository
- Triggers ArgoCD sync

### Setting Up GitHub Secrets

Navigate to your repository → **Settings** → **Secrets and variables** → **Actions**, and add:

| Secret Name | Description |
|-------------|-------------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub access token |
| `TOKEN` | GitHub Personal Access Token (classic) with `repo` scope |

**Generate Docker Hub Token:**
1. Go to Docker Hub → Account Settings → Security
2. Click "New Access Token"
3. Copy and save the token

**Generate GitHub Token:**
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token with `repo` scope
3. Copy and save the token

## ☸️ Kubernetes Deployment

### Step 1: Create EKS Cluster

```bash
# Configure AWS CLI
aws configure

# Create EKS cluster
eksctl create cluster --name demo-cluster --region us-east-1

# Update kubeconfig
aws eks update-kubeconfig --name demo-cluster --region us-east-1
```

### Step 2: Install NGINX Ingress Controller

```bash
# Install NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.14.2/deploy/static/provider/aws/deploy.yaml

# Verify installation
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```

### Step 3: Deploy with Helm

```bash
# Install the Helm chart
helm install go-web-app ./helm/go-web-app-chart

# Verify deployment
kubectl get all
kubectl get ingress
```

### Step 4: Configure Local DNS (Optional)

Get the Load Balancer DNS:

```bash
kubectl get ingress
```

Get the Load Balancer IP:

```bash
nslookup <LOAD_BALANCER_DNS>
```

**On Windows (as Administrator):**

```powershell
notepad C:\Windows\System32\drivers\etc\hosts
```

Add line:
```
<LOAD_BALANCER_IP> go-web-app.local
```

**On macOS/Linux:**

```bash
sudo vi /etc/hosts
```

Add line:
```
<LOAD_BALANCER_IP> go-web-app.local
```

Access the application at `http://go-web-app.local`

## 🔄 GitOps with ArgoCD

### Step 1: Install ArgoCD

```bash
# Create namespace
kubectl create namespace argocd

# Install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Expose ArgoCD server via LoadBalancer
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

# Get ArgoCD server URL
kubectl get svc argocd-server -n argocd
```

### Step 2: Access ArgoCD UI

```bash
# Get the initial admin password
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

**Login Credentials:**
- **Username:** `admin`
- **Password:** (decoded password from above)

Access ArgoCD at the LoadBalancer external IP.

### Step 3: Create Application in ArgoCD

**Via UI:**

1. Click **+ NEW APP**
2. Fill in the following:
   - **Application Name:** `go-web-app`
   - **Project Name:** `default`
   - **Sync Policy:** `Automatic`
   - **Enable Auto-Sync:** ✅
   - **Self Heal:** ✅
   - **Repository URL:** `https://github.com/ToBeaBlessing/go-web-app.git`
   - **Revision:** `HEAD`
   - **Path:** `helm/go-web-app-chart`
   - **Cluster URL:** `https://kubernetes.default.svc`
   - **Namespace:** `default`
3. Click **CREATE**

**Via CLI:**

```bash
argocd app create go-web-app \
  --repo https://github.com/ToBeaBlessing/go-web-app.git \
  --path helm/go-web-app-chart \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default \
  --sync-policy automated \
  --auto-prune \
  --self-heal
```

### Step 4: Verify Sync Status

```bash
# Check ArgoCD application status
kubectl get applications -n argocd

# View deployment in default namespace
kubectl get deploy,svc,ingress
```

## 🧪 Testing the Pipeline

### End-to-End Test

1. **Make a code change:**

```bash
# Edit static/home.html
vi static/home.html
# Update the header or content
```

2. **Commit and push:**

```bash
git add .
git commit -m "Update homepage header"
git push
```

3. **Monitor the pipeline:**

- **GitHub Actions:** Check the Actions tab in your repository
- **Docker Hub:** Verify new image with updated tag
- **ArgoCD:** Watch automatic sync in ArgoCD UI
- **Application:** Refresh your browser to see changes

### Pipeline Validation Checklist

- [ ] GitHub Actions workflow completes successfully
- [ ] New Docker image appears in Docker Hub with run ID tag
- [ ] `helm/go-web-app-chart/values.yaml` updated with new tag
- [ ] ArgoCD detects change and syncs
- [ ] Application pods restart with new image
- [ ] Changes visible in browser

## 🧹 Cleanup

### Delete ArgoCD Application

```bash
# Via CLI
argocd app delete go-web-app

# Via kubectl
kubectl delete application go-web-app -n argocd
```

### Uninstall Helm Release

```bash
helm uninstall go-web-app
```

### Delete EKS Cluster

```bash
eksctl delete cluster --name demo-cluster --region us-east-1
```

### Delete ArgoCD

```bash
kubectl delete namespace argocd
```

## 💼 Skills Demonstrated

This project showcases expertise in:

### DevOps Practices
- ✅ **CI/CD Pipelines:** GitHub Actions with multi-stage workflows
- ✅ **GitOps:** Declarative infrastructure with ArgoCD
- ✅ **Infrastructure as Code:** Kubernetes manifests and Helm charts
- ✅ **Container Security:** Multi-stage builds and distroless images

### Cloud & Kubernetes
- ☁️ **AWS Services:** EKS, Load Balancers, VPC
- 🐳 **Container Orchestration:** Kubernetes deployments, services, ingress
- 📦 **Package Management:** Helm for environment configuration
- 🔄 **Continuous Deployment:** Automated rollouts with ArgoCD

### Software Engineering
- 🔧 **Go Development:** Web server implementation
- 🧪 **Testing:** Unit tests and code quality checks
- 🔐 **Security:** Secrets management, least-privilege access
- 📊 **Monitoring:** Application health checks and readiness probes

## 🔧 Troubleshooting

### Build Failures

**Issue:** GitHub Actions build fails

**Solution:**
```bash
# Test locally first
go build -o go-web-app
go test ./...

# Check go.mod and go.sum are committed
git add go.mod go.sum
git commit -m "Add Go dependencies"
git push
```

### Docker Push Fails

**Issue:** Authentication error pushing to Docker Hub

**Solution:**
1. Verify `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` secrets
2. Regenerate Docker Hub token if expired
3. Ensure Docker Hub repository exists

### Helm Update Fails

**Issue:** `Error: Input required and not supplied: token`

**Solution:**
1. Create GitHub Personal Access Token (classic)
2. Select `repo` scope
3. Add as `TOKEN` secret in repository settings

### ArgoCD Not Syncing

**Issue:** ArgoCD shows "OutOfSync" status

**Solution:**
```bash
# Manual sync
kubectl patch application go-web-app -n argocd --type merge -p '{"operation": {"sync": {}}}'

# Check ArgoCD application logs
kubectl logs -n argocd deployment/argocd-application-controller
```

### Ingress Not Working

**Issue:** Cannot access application via ingress

**Solution:**
```bash
# Verify NGINX Ingress Controller is running
kubectl get pods -n ingress-nginx

# Check ingress configuration
kubectl describe ingress go-web-app

# Verify service endpoints
kubectl get endpoints go-web-app
```

## 📚 Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
- [Helm Best Practices](https://helm.sh/docs/chart_best_practices/)
- [Kubernetes Ingress Documentation](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [Docker Multi-Stage Builds](https://docs.docker.com/build/building/multi-stage/)
- [AWS EKS Best Practices](https://aws.github.io/aws-eks-best-practices/)

## 🚀 Future Enhancements

- [ ] Add Prometheus and Grafana for monitoring
- [ ] Implement horizontal pod autoscaling (HPA)
- [ ] Add SSL/TLS certificates with cert-manager
- [ ] Implement blue-green deployments
- [ ] Add integration tests in CI pipeline
- [ ] Configure network policies for security
- [ ] Add Slack notifications for deployments
- [ ] Implement canary deployments with Argo Rollouts
- [ ] Add image vulnerability scanning with Trivy
- [ ] Configure backup and disaster recovery

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Author:** Blessing Omomola  
**Institution:** Morgan State University - Industrial Engineering (MS)  
**GitHub:** [@ToBeaBlessing](https://github.com/ToBeaBlessing)  
**LinkedIn:** [Connect with me](https://linkedin.com/in/yourprofile)

⭐ If this project helped you learn DevOps practices, please star it!
