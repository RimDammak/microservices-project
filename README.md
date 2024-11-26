#  **Microservices Application Deployment with Terraform & Kubernetes**  

## **Project Overview**  
This project demonstrates the deployment of a scalable microservices application on Kubernetes using Terraform for infrastructure automation. It incorporates DevOps best practices, GitOps principles, and modern cloud-native tools to ensure automation, observability, and security.

---

## **Key Features**  
1. **Infrastructure Provisioning**:  
   - Azure-based infrastructure managed with Terraform.  
   - Resources include:  
     - AKS (Azure Kubernetes Service)  
     - ACR (Azure Container Registry)  
     - Blob Storage for Terraform state  
     - Virtual Network (VNET) with NSG for secure networking  

2. **Microservices Architecture**:  
   - Core services:  
     - User, Post, Connections, Notification, Discovery Service, API Gateway  
   - Base services:  
     - MySQL, Redis, Kafka, Zookeeper  

3. **Workload Organization**:  
   - Four Kubernetes namespaces:  
     - `default`: Application services  
     - `devops`: CI/CD tools (e.g., Jenkins, ArgoCD)  
     - `monitoring`: Prometheus & Grafana  
     - `vault`: HashiCorp Vault  

4. **CI/CD and GitOps**:  
   - Jenkins and GitLab for pipeline automation.  
   - ArgoCD for GitOps-based continuous deployment.  

5. **Monitoring & Security**:  
   - Observability with Prometheus & Grafana.  
   - Secrets management with HashiCorp Vault.  
   - HTTPS traffic routing with cert-manager and Ingress-NGINX.  

---

## **Technologies Used**  
- **Infrastructure as Code (IaC)**: Terraform  
- **Containerization**: Docker  
- **Orchestration**: Kubernetes, Helm  
- **CI/CD Tools**: Jenkins, GitLab, ArgoCD  
- **Monitoring**: Prometheus, Grafana  
- **Secrets Management**: HashiCorp Vault  
- **Application Framework**: Spring Boot  

---

## **Setup Instructions**  

### **Phase 1: Environment Setup**  
1. Install the following tools:  
   - **Terraform**: [Install Guide](https://learn.hashicorp.com/tutorials/terraform/install-cli)  
   - **kubectl**: [Install Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/)  
   - **Docker**: [Install Guide](https://docs.docker.com/get-docker/)  
   - **Helm**: [Install Guide](https://helm.sh/docs/intro/install/)  
   - **Azure CLI**: [Install Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)  
2. Authenticate Azure CLI with your Azure account:  
   ```bash  
   az login  
   ```

---

### **Phase 2: Infrastructure Provisioning**  
1. Clone the Terraform configuration repository:  
   ```bash  
   git clone https://github.com/your-repo/terraform-aks-setup.git  
   cd terraform-aks-setup  
   ```  
2. Initialize and apply Terraform:  
   ```bash  
   terraform init  
   terraform apply  
   ```  
3. Verify the creation of resources:  
   - AKS cluster  
   - ACR  
   - Blob Storage  
   - Networking components  

---

### **Phase 3: Application Containerization**  
1. Containerize each microservice:  
   - Write Dockerfiles for each Spring Boot service.  
   - Build and push images to ACR:  
     ```bash  
     docker build -t <acr-name>.azurecr.io/<service-name>:<tag> .  
     docker push <acr-name>.azurecr.io/<service-name>:<tag>  
     ```  

---

### **Phase 4: Kubernetes Deployment**  
1. Create Kubernetes namespaces:  
   ```bash  
   kubectl create namespace default  
   kubectl create namespace devops  
   kubectl create namespace monitoring  
   kubectl create namespace vault  
   ```  
2. Deploy microservices using Helm:  
   ```bash  
   helm install <release-name> ./helm-charts/<service-name> -n default  
   ```  

---

### **Phase 5: CI/CD Pipeline Setup**  
1. Configure Jenkins pipelines for build and deploy stages.  
2. Use GitLab for version control and integration with CI/CD pipelines.  
3. Install and configure ArgoCD for GitOps-based deployments:  
   ```bash  
   kubectl apply -n devops -f argocd-install.yaml  
   ```  

---

### **Phase 6: Monitoring and Security**  
1. Deploy Prometheus and Grafana in the `monitoring` namespace:  
   ```bash  
   helm install prometheus ./helm-charts/prometheus -n monitoring  
   helm install grafana ./helm-charts/grafana -n monitoring  
   ```  
2. Set up HashiCorp Vault for secrets management:  
   ```bash  
   helm install vault ./helm-charts/vault -n vault  
   ```  
3. Enable HTTPS with cert-manager and Ingress-NGINX.  

---

### **Phase 7: Testing and Validation**  
1. Validate CI/CD pipelines and test deployment workflows.  
2. Check monitoring dashboards and set up alert rules.  
3. Access the application through Ingress with HTTPS enabled.  

---

## **Expected Outcomes**  
1. Automated infrastructure provisioning using Terraform.  
2. Containerized microservices deployed on Kubernetes with Helm.  
3. Robust CI/CD pipelines and GitOps workflows.  
4. Monitoring dashboards with Prometheus & Grafana.  
5. Secure secrets management with HashiCorp Vault.  

---

## **Learnings and Challenges**  
- Gained hands-on experience in Terraform and Kubernetes.  
- Learned CI/CD and GitOps practices with Jenkins, GitLab, and ArgoCD.  
- Overcame challenges in scaling microservices and securing sensitive data.  

---

## **Future Improvements**  
- Introduce auto-scaling policies for microservices.  
- Implement advanced monitoring with distributed tracing tools.  
- Explore multi-cloud deployments for enhanced resilience.  

---


## **Contributors**  
This project was lovingly crafted by:  
- **Rim Dammak** - [📧 dammakrim2003@gmail.com](mailto:dammakrim2003@gmail.com)  
- **Sayf Ben Sallah** - [📧 sayfeddinebensalah@outlook.com](mailto:sayfeddinebensalah@outlook.com)  

We came, we saw, we Terraform-ed. 🚀

---

