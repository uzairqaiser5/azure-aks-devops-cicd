# ☁️ Azure Kubernetes CI/CD & Monitoring Project

> **End-to-end Azure DevOps CI/CD pipeline for deploying a containerized application to Azure Kubernetes Service (AKS), storing images in Azure Container Registry (ACR), and automatically installing Prometheus & Grafana monitoring with Helm.**

![Azure](https://img.shields.io/badge/Microsoft%20Azure-Cloud-0078D4?logo=microsoftazure&logoColor=white)
![AKS](https://img.shields.io/badge/Azure-AKS-0078D4?logo=microsoftazure&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure-DevOps-0078D4?logo=azuredevops&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?logo=kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-Package%20Manager-0F1689?logo=helm&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-F46800?logo=grafana&logoColor=white)

---

## 📌 Project Overview

The purpose of this project was to build a **complete automated cloud deployment workflow** using Microsoft Azure and Azure DevOps.

The project starts with a containerized application stored in GitHub and continues through the complete CI/CD lifecycle:

**GitHub → Azure DevOps → Docker Build → Azure Container Registry → AKS → Kubernetes Deployment → Helm → Prometheus → Grafana**

The main objective was to automate the deployment process as much as possible so that a code change pushed to the `main` branch can trigger the pipeline, build a new Docker image, push it to Azure Container Registry, deploy the updated application to Azure Kubernetes Service, and deploy the monitoring stack using Helm.

This project was created as a **hands-on Microsoft Azure / DevOps / Kubernetes learning project and portfolio demonstration**.

---

# 🎯 Project Objectives

The project was designed around the following objectives:

- ☁️ Create and manage an **Azure Kubernetes Service (AKS)** cluster using Azure CLI.
- 🐳 Containerize and build the application using Docker.
- 📦 Create **Azure Container Registry (ACR)** for storing application images.
- 🔗 Connect **GitHub with Azure DevOps Pipelines**.
- ⚙️ Automate Docker image build and push operations.
- 🚀 Automatically deploy the application to AKS.
- ☸️ Use Kubernetes `Deployment` and `Service` manifests.
- 📦 Use **Helm** to automate Kubernetes package deployment.
- 📊 Deploy **Prometheus & Grafana** for monitoring.
- 🔄 Create a complete CI/CD workflow from source code to running application.
- 🧪 Practice Azure CLI, Kubernetes, Docker, Helm and Azure DevOps together.

---

# 🏗️ Architecture

```text
                    ┌──────────────────────┐
                    │       GitHub         │
                    │   Application Code   │
                    └──────────┬───────────┘
                               │
                               │ Git Push
                               ▼
                    ┌──────────────────────┐
                    │    Azure DevOps      │
                    │      Pipeline        │
                    └──────────┬───────────┘
                               │
                    ┌──────────▼───────────┐
                    │      Docker Build    │
                    │   Build Application   │
                    └──────────┬───────────┘
                               │
                               │ Push Image
                               ▼
              ┌────────────────────────────────┐
              │ Azure Container Registry (ACR) │
              │      registry7967.azurecr.io   │
              └───────────────┬────────────────┘
                              │
                              │ Pull Image
                              ▼
                    ┌──────────────────────┐
                    │ Azure Kubernetes     │
                    │ Service (AKS)        │
                    │                      │
                    │ ┌──────────────────┐ │
                    │ │ Application Pods │ │
                    │ └──────────────────┘ │
                    │          │           │
                    │     Kubernetes       │
                    │     Service          │
                    └──────────┬───────────┘
                               │
                    ┌──────────▼───────────┐
                    │        Helm          │
                    │ Package Deployment   │
                    └──────────┬───────────┘
                               │
                    ┌──────────▼───────────┐
                    │ Prometheus + Grafana │
                    │      Monitoring      │
                    └──────────────────────┘
```

---

# 🔄 End-to-End CI/CD Flow

```text
1. Developer pushes code to GitHub
                    ↓
2. Azure DevOps detects change
                    ↓
3. Pipeline starts automatically
                    ↓
4. Docker image is built
                    ↓
5. Image is pushed to Azure Container Registry
                    ↓
6. Kubernetes deployment is updated
                    ↓
7. AKS pulls the new image from ACR
                    ↓
8. Application becomes available through Kubernetes Service
                    ↓
9. Helm installs / updates monitoring stack
                    ↓
10. Prometheus collects metrics
                    ↓
11. Grafana provides monitoring dashboards
```

---

# 🛠️ Azure Services & Technologies Used

| Technology | Purpose |
|---|---|
| **Azure Kubernetes Service (AKS)** | Kubernetes cluster for application deployment |
| **Azure Container Registry (ACR)** | Private container image registry |
| **Azure DevOps Pipelines** | CI/CD automation |
| **GitHub** | Source code repository |
| **Docker** | Application containerization |
| **Kubernetes** | Container orchestration |
| **Helm** | Kubernetes package management |
| **Prometheus** | Metrics collection and monitoring |
| **Grafana** | Monitoring dashboards and visualization |
| **Azure CLI** | Azure resource management |
| **kubectl** | Kubernetes cluster management |
| **YAML** | Pipeline and Kubernetes configuration |

---

# 1️⃣ Create the AKS Test / Development Cluster

The first step was creating an **Azure Kubernetes Service (AKS)** cluster for the development/testing environment.

## 🔐 Login to Azure

```bash
az login
```

Check the active subscription:

```bash
az account show
```

If multiple subscriptions are available:

```bash
az account list --output table
```

Select the required subscription:

```bash
az account set --subscription "<SUBSCRIPTION_ID>"
```

---

## 📦 Create Resource Group

Example:

```bash
az group create \
  --name <RESOURCE_GROUP_NAME> \
  --location eastus
```

> Replace `<RESOURCE_GROUP_NAME>` with your own resource group.

---

## ☸️ Create AKS Cluster

Example:

```bash
az aks create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name <AKS_CLUSTER_NAME> \
  --node-count 2 \
  --enable-managed-identity \
  --generate-ssh-keys
```

Verify the cluster:

```bash
az aks show \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name <AKS_CLUSTER_NAME> \
  --output table
```

### 📸 Screenshot — AKS Cluster Creation

Replace the placeholder below with your screenshot:

```text
![AKS Cluster Created](screenshots/01-aks-cluster-created.png)
```

---

# 2️⃣ Connect kubectl to AKS

After creating the cluster, I configured the local Kubernetes client to communicate with AKS.

```bash
az aks get-credentials \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name <AKS_CLUSTER_NAME>
```

Verify the connection:

```bash
kubectl get nodes
```

Expected output will show the AKS worker nodes.

```bash
kubectl get nodes -o wide
```

### 📸 Screenshot — AKS Cluster

<img width="1835" height="488" alt="image" src="https://github.com/user-attachments/assets/30496a64-e711-42ff-b7e5-2ceb23685723" />


---

# 3️⃣ Configure Kubernetes Permissions for Azure DevOps

For this learning project, the Azure DevOps deployment identity/service account required permissions to deploy resources to the Kubernetes cluster.

The configuration used during the project was:

```bash
kubectl create clusterrolebinding devops-default-sa-admin \
  --clusterrole=cluster-admin \
  --group=system:serviceaccounts:default
```

This was used to allow the service accounts in the `default` namespace to perform deployment operations.

### ⚠️ Security Note

`cluster-admin` provides very broad permissions and should **not** normally be used in a production environment.

For production, a better approach would be to create a dedicated service account and assign only the Kubernetes permissions required by the CI/CD pipeline using **RBAC / Role / RoleBinding**.

---

# 4️⃣ Create Azure Container Registry (ACR)

The next step was creating an **Azure Container Registry** to store Docker images generated by the Azure DevOps pipeline.

The registry used in this project was:

```text
registry7967.azurecr.io
```

Create an ACR:

```bash
az acr create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name registry7967 \
  --sku Basic
```

Verify:

```bash
az acr show \
  --name registry7967 \
  --resource-group <RESOURCE_GROUP_NAME> \
  --output table
```

List repositories:

```bash
az acr repository list \
  --name registry7967 \
  --output table
```

The application image repository used by the pipeline was:

```text
githubweb1
```

The final image format was:

```text
registry7967.azurecr.io/githubweb1:<BUILD_ID>
```

### 📸 Screenshot — Azure Container Registry
<img width="1694" height="817" alt="image" src="https://github.com/user-attachments/assets/c72fa55d-23ff-4d68-a257-eeb6e9cc4a83" />


# 5️⃣ Connect GitHub with Azure DevOps

The application source code was maintained in GitHub.

Azure DevOps was then configured to use the GitHub repository as the source for the CI/CD pipeline.

The pipeline was configured to trigger whenever changes were pushed to:

```text
main
```

Pipeline trigger:

```yaml
trigger:
- main
```

This means a push to the `main` branch can automatically start the CI/CD process.

### 📸 Screenshot — GitHub Repository

```text
![GitHub Repository](screenshots/05-github-repository.png)
```

### 📸 Screenshot — Azure DevOps Pipeline

<img width="1913" height="833" alt="image" src="https://github.com/user-attachments/assets/84e8b2dd-8df5-4795-b6f4-4f00799cb178" />


# 6️⃣ Azure DevOps Service Connection

An Azure DevOps service connection was configured to allow the pipeline to communicate with the required Azure/Kubernetes resources.

The pipeline uses a Docker registry service connection:

```yaml
dockerRegistryServiceConnection: '<AZURE_DEVOPS_SERVICE_CONNECTION_ID>'
```

> **Security:** The real service connection ID is intentionally represented as a placeholder in this public portfolio README. The actual value should remain inside Azure DevOps and should not be treated as application configuration.

---

# 7️⃣ Kubernetes Deployment Files

The pipeline deploys the Kubernetes manifests from the `manifests` directory.

Example project structure:

```text
.
├── Dockerfile
├── Azure-pipeline.yml
├── manifests/
│   ├── deployment.yml
│   └── service.yml
├── screenshots/
│   ├── 01-aks-cluster-created.png
│   ├── 02-aks-nodes.png
│   ├── 03-kubernetes-permissions.png
│   ├── 04-acr-created.png
│   ├── 05-github-repository.png
│   ├── 06-azure-devops-pipeline.png
│   ├── 07-pipeline-build.png
│   ├── 08-acr-image.png
│   ├── 09-aks-application.png
│   ├── 10-prometheus.png
│   └── 11-grafana.png
└── README.md
```

---

# 8️⃣ Azure DevOps CI/CD Pipeline

The pipeline contains two main stages:

```text
Build
  ↓
Deploy
```

## Build Stage

The Build stage:

1. Uses an Ubuntu build agent.
2. Builds the Docker image.
3. Pushes the image to ACR.
4. Publishes Kubernetes manifests as a pipeline artifact.

---

## Deploy Stage

The Deploy stage:

1. Creates the Kubernetes image pull secret.
2. Deploys the application manifests.
3. Replaces the container image with the newly built ACR image.
4. Installs Helm.
5. Adds the Prometheus Community Helm repository.
6. Deploys `kube-prometheus-stack`.
7. Enables Grafana through a LoadBalancer service.

---

# 📄 Complete `Azure-pipeline.yml`

The following is the cleaned portfolio version of the pipeline used for this project.

> **Important:** The actual Azure DevOps service connection ID and Grafana password should be stored securely in Azure DevOps rather than committed to GitHub.

```yaml
# Deploy to Azure Kubernetes Service
# Build and push image to Azure Container Registry
# Deploy application to Azure Kubernetes Service
# Install Prometheus & Grafana using Helm

trigger:
- main

resources:
- repo: self

variables:
  dockerRegistryServiceConnection: '<AZURE_DEVOPS_SERVICE_CONNECTION_ID>'
  imageRepository: 'githubweb1'
  containerRegistry: 'registry7967.azurecr.io'
  dockerfilePath: '**/Dockerfile'
  tag: '$(Build.BuildId)'
  imagePullSecret: 'registry79672f31-auth'
  vmImageName: 'ubuntu-latest'

stages:

# =========================================================
# BUILD STAGE
# =========================================================

- stage: Build
  displayName: Build stage

  jobs:
  - job: Build
    displayName: Build

    pool:
      vmImage: $(vmImageName)

    steps:

    # Build Docker image and push it to Azure Container Registry
    - task: Docker@2
      displayName: Build and push an image to container registry

      inputs:
        command: buildAndPush
        repository: $(imageRepository)
        dockerfile: $(dockerfilePath)
        containerRegistry: $(dockerRegistryServiceConnection)

        tags: |
          $(tag)

    # Publish Kubernetes manifests
    - upload: manifests
      artifact: manifests


# =========================================================
# DEPLOY STAGE
# =========================================================

- stage: Deploy
  displayName: Deploy stage

  dependsOn: Build

  jobs:

  - deployment: Deploy
    displayName: Deploy

    pool:
      vmImage: $(vmImageName)

    environment: 'uzairqaiser5staticapp1.default'

    strategy:

      runOnce:

        deploy:

          steps:

          # -------------------------------------------------
          # Step 1: Create Kubernetes Image Pull Secret
          # -------------------------------------------------

          - task: KubernetesManifest@0

            displayName: Create imagePullSecret

            inputs:
              action: createSecret
              secretName: $(imagePullSecret)
              dockerRegistryEndpoint: $(dockerRegistryServiceConnection)


          # -------------------------------------------------
          # Step 2: Deploy Application to AKS
          # -------------------------------------------------

          - task: KubernetesManifest@0

            displayName: Deploy to Kubernetes cluster

            inputs:

              action: deploy

              manifests: |
                $(Pipeline.Workspace)/manifests/deployment.yml
                $(Pipeline.Workspace)/manifests/service.yml

              imagePullSecrets: |
                $(imagePullSecret)

              containers: |
                $(containerRegistry)/$(imageRepository):$(tag)


          # -------------------------------------------------
          # Step 3: Install Helm
          # -------------------------------------------------

          - task: HelmInstaller@1

            displayName: 'Install Helm'

            inputs:
              helmVersionToInstall: 'latest'


          # -------------------------------------------------
          # Step 4: Deploy Prometheus & Grafana
          # -------------------------------------------------

          - bash: |

              # Find Kubernetes kubeconfig
              CONFIG_FILE=$(find $(Agent.TempDirectory) \
                -name "config" \
                -o -name "kubeconfig" | head -n 1)

              if [ -n "$CONFIG_FILE" ]; then
                export KUBECONFIG="$CONFIG_FILE"
              fi


              # Add Prometheus Community repository
              helm repo add prometheus-community \
                https://prometheus-community.github.io/helm-charts


              # Update Helm repositories
              helm repo update


              # Install / Upgrade Prometheus + Grafana
              helm upgrade --install monitoring-stack \
                prometheus-community/kube-prometheus-stack \
                --namespace default \
                --set grafana.service.type=LoadBalancer \
                --set grafana.adminPassword="$(GRAFANA_ADMIN_PASSWORD)"

            displayName: 'Deploy Prometheus and Grafana via Helm'
```

---

# 🔐 Recommended Azure DevOps Secret

The original learning pipeline contained a Grafana administrator password directly in YAML.

For a public GitHub portfolio, this should **not** be committed.

Instead, create an Azure DevOps secret variable:

```text
GRAFANA_ADMIN_PASSWORD
```

Then reference it in the pipeline:

```yaml
--set grafana.adminPassword="$(GRAFANA_ADMIN_PASSWORD)"
```

Mark the variable as:

```text
Keep this value secret
```

This keeps credentials outside the GitHub repository.

---

# 9️⃣ Build & Push Docker Image

During the Build stage, Azure DevOps uses the Docker task:

```yaml
- task: Docker@2
  displayName: Build and push an image to container registry
```

The generated image is tagged using the Azure DevOps build ID:

```yaml
tag: '$(Build.BuildId)'
```

The resulting image follows this format:

```text
registry7967.azurecr.io/githubweb1:<BUILD_ID>
```

This gives each pipeline build a unique image tag.

### 📸 Screenshot — Successful Build

```text
![Azure DevOps Build](screenshots/07-pipeline-build.png)
```

---

# 🔟 Verify Image in Azure Container Registry

After a successful build, the image can be verified using Azure CLI:

```bash
az acr repository list \
  --name registry7967 \
  --output table
```

Show image tags:

```bash
az acr repository show-tags \
  --name registry7967 \
  --repository githubweb1 \
  --output table
```

Example:

```text
githubweb1
└── 123
└── 124
└── 125
```

The build ID changes with each successful pipeline run.

### 📸 Screenshot — ACR Image

<img width="1778" height="891" alt="image" src="https://github.com/user-attachments/assets/11c134b1-6e9f-4548-a8af-7ea8bd9d1b71" />


# 1️⃣1️⃣ Deploy Application to AKS

The pipeline automatically deploys:

```text
deployment.yml
service.yml
```

The image is dynamically replaced with the newly generated ACR image:

```yaml
containers: |
  $(containerRegistry)/$(imageRepository):$(tag)
```

This means the pipeline does not need a manually updated image tag for every deployment.

---

# 🔍 Verify Kubernetes Deployment

After deployment:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get services
```

For more details:

```bash
kubectl get pods -o wide
```

Check deployment status:

```bash
kubectl rollout status deployment/<DEPLOYMENT_NAME>
```

### 📸 Screenshot — Running Application Pods

<img width="1869" height="937" alt="image" src="https://github.com/user-attachments/assets/1eb3ee95-1501-4065-9d77-57122b978759" />

---

# 1️⃣2️⃣ Helm Monitoring Stack

The project also automates monitoring deployment using Helm.

The pipeline adds the Prometheus Community repository:

```bash
helm repo add prometheus-community \
  https://prometheus-community.github.io/helm-charts

helm repo update
```

Then installs:

```text
kube-prometheus-stack
```

using:

```bash
helm upgrade --install monitoring-stack \
  prometheus-community/kube-prometheus-stack \
  --namespace default \
  --set grafana.service.type=LoadBalancer
```

The stack provides components including:

```text
Prometheus
Grafana
Alertmanager
Node Exporter
Kube State Metrics
```

---

# 1️⃣3️⃣ Verify Prometheus & Grafana

Check Helm releases:

```bash
helm list -n default
```

Check monitoring pods:

```bash
kubectl get pods -n default
```

Check services:

```bash
kubectl get svc -n default
```

Because Grafana was configured as a `LoadBalancer`, retrieve the external IP:

```bash
kubectl get svc monitoring-stack-grafana -n default
```

You can also inspect all services:

```bash
kubectl get svc -n default
```

### 📸Application Running 

<img width="1834" height="878" alt="image" src="https://github.com/user-attachments/assets/8c778a37-d3f9-48dd-8ec3-1e7bd328b9f8" />



### 📸 Screenshot — Grafana Dashboard

<img width="1913" height="954" alt="image" src="https://github.com/user-attachments/assets/8546bc97-d49f-4ab9-82c4-4283ce476a61" />


---

# 📊 Monitoring Architecture

```text
              ┌─────────────────────────┐
              │       AKS Cluster       │
              │                         │
              │   Application Pods      │
              │          │              │
              │          ▼              │
              │      Kubernetes         │
              │       Metrics           │
              └──────────┬──────────────┘
                         │
                         ▼
                ┌─────────────────┐
                │   Prometheus    │
                │ Metrics Storage │
                └────────┬────────┘
                         │
                         ▼
                ┌─────────────────┐
                │     Grafana     │
                │    Dashboards   │
                └─────────────────┘
```

---

# 🔁 Complete Automated Workflow

Once the infrastructure and Azure DevOps pipeline were configured, the complete workflow became:

```text
                 GitHub
                    │
                    │ Push to main
                    ▼
            ┌───────────────┐
            │ Azure DevOps  │
            │   Pipeline    │
            └───────┬───────┘
                    │
                    ▼
             Docker Build
                    │
                    ▼
            Azure Container
               Registry
                    │
                    ▼
                 AKS
                    │
                    ▼
          Kubernetes Deployment
                    │
                    ▼
              Application
                    │
                    ▼
                 Helm
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
     Prometheus            Grafana
     Monitoring            Dashboard
```

The key goal was to move from a manual deployment process toward an automated workflow where the pipeline handles the major deployment and monitoring steps.

---

# 🧪 Useful Verification Commands

## Azure

```bash
az account show
az group list --output table
az aks list --output table
az acr list --output table
```

## AKS

```bash
az aks get-credentials \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name <AKS_CLUSTER_NAME>

kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl get services
```

## ACR

```bash
az acr repository list \
  --name registry7967 \
  --output table

az acr repository show-tags \
  --name registry7967 \
  --repository githubweb1 \
  --output table
```

## Helm

```bash
helm list -n default
helm repo list
```

## Monitoring

```bash
kubectl get pods -n default
kubectl get svc -n default
kubectl get pods -l "app.kubernetes.io/instance=monitoring-stack"
```

---



# 🧠 What I Learned

This project gave me practical experience with:

### Azure

- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Azure CLI
- Azure resource groups
- Azure DevOps
- Azure DevOps service connections

### DevOps

- CI/CD concepts
- GitHub integration
- YAML pipelines
- Automated Docker builds
- Container image versioning
- Automated Kubernetes deployment

### Containers

- Docker
- Docker images
- Azure Container Registry
- Kubernetes image pull secrets

### Kubernetes

- AKS
- Pods
- Deployments
- Services
- Namespaces
- RBAC
- Kubernetes manifests
- `kubectl`

### Helm & Monitoring

- Helm repositories
- Helm chart deployment
- `kube-prometheus-stack`
- Prometheus
- Grafana
- Kubernetes monitoring

---

# 🚀 Future Improvements

Possible next improvements for this project include:

- [ ] Implement production-grade Kubernetes RBAC instead of `cluster-admin`.
- [ ] Store all secrets in **Azure Key Vault**.
- [ ] Use Azure DevOps Variable Groups for configuration.
- [ ] Add separate `dev`, `staging`, and `production` environments.
- [ ] Add automated unit/integration testing before deployment.
- [ ] Add security scanning for Docker images.
- [ ] Add Terraform or Bicep for Infrastructure as Code.
- [ ] Add Helm charts for the application itself.
- [ ] Configure HTTPS with TLS certificates.
- [ ] Add Azure Application Gateway / Ingress.
- [ ] Add Prometheus alerting and Grafana alert rules.
- [ ] Implement GitOps using Argo CD or Flux.
- [ ] Add horizontal pod autoscaling (HPA).

---

# 🔐 Security Considerations

This repository is intended as a **learning and portfolio project**.

For a production implementation:

- Do not commit passwords or tokens to GitHub.
- Do not expose Azure DevOps secrets in YAML.
- Use Azure Key Vault for sensitive information.
- Use managed identities wherever possible.
- Use dedicated Kubernetes service accounts.
- Follow least-privilege RBAC.
- Avoid `cluster-admin` unless absolutely necessary.
- Restrict public LoadBalancer access.
- Enable HTTPS/TLS.
- Scan container images for vulnerabilities.
- Separate development and production environments.

---

# 📚 Project Summary

This project demonstrates an end-to-end cloud-native deployment workflow using Microsoft Azure.

The final solution connects:

```text
GitHub
   ↓
Azure DevOps
   ↓
Docker
   ↓
Azure Container Registry
   ↓
Azure Kubernetes Service
   ↓
Kubernetes
   ↓
Helm
   ↓
Prometheus
   ↓
Grafana
```

The main learning objective was to understand how modern DevOps tools can work together to automate the journey from **source code → container image → Kubernetes deployment → monitoring**.

---

# 👨‍💻 Portfolio Project

**Project Type:** Microsoft Azure / DevOps / Kubernetes Learning Project

**Core Focus:**

> **Automating application deployment and monitoring on Azure using CI/CD, containers, Kubernetes and Helm.**

---

⭐ If you found this project useful, feel free to explore the repository and the implementation files.

#  Uzair - Senior Cloud & Devops Engineer
#  Email - uzairqaiser5@gmail.com
# Linkedin- @uzairqaiser5



