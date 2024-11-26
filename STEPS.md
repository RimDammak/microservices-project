# **STEPS.md**  
## **Microservices Application Deployment with Terraform & Kubernetes**  

### **Step-by-Step Guide**

---

### **Step 1: Environment Setup**  
1. **Install Prerequisite Tools**:  
   - Install the following tools on your local machine:  
     - [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)  
     - [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)  
     - [Docker](https://docs.docker.com/get-docker/)  
     - [Helm](https://helm.sh/docs/intro/install/)  
     - [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)  
   - Authenticate Azure CLI with your account:  
     ```bash  
     az login  
     ```  

2. **Set Up Azure Environment**:  
   - Ensure you have an Azure subscription.  
   - Create a new resource group for your project:  
     ```bash  
     az group create --name <resource-group-name> --location <region>  
     ```  

---

### **Step 2: Infrastructure Provisioning with Terraform**  
1. **Clone the Terraform Repository**:  
   ```bash  
   git clone https://github.com/your-repo/terraform-aks-setup.git  
   cd terraform-aks-setup  
   ```  

2. **Configure Terraform Backend**:  
   - Update the `backend` block in the `main.tf` file to point to your Azure Blob Storage for state storage.  

3. **Initialize and Apply Terraform**:  
   ```bash  
   terraform init  
   terraform plan  
   terraform apply  
   ```  

4. **Verify Infrastructure**:  
   - Confirm that the following Azure resources have been provisioned:  
     - AKS (Azure Kubernetes Service)  
     - ACR (Azure Container Registry)  
     - Blob Storage  

---

### **Step 3: Containerize Application**  
1. **Write Dockerfiles**:  
   - For each Spring Boot microservice, create a `Dockerfile`.  
   - Example `Dockerfile` for a Spring Boot service:  
     ```dockerfile  
     FROM openjdk:11-jdk  
     COPY target/<service-name>.jar app.jar  
     ENTRYPOINT ["java", "-jar", "/app.jar"]  
     ```  

2. **Build and Push Images**:  
   - Build Docker images for each microservice:  
     ```bash  
     docker build -t <acr-name>.azurecr.io/<service-name>:<tag> .  
     ```  
   - Push images to ACR:  
     ```bash  
     az acr login --name <acr-name>  
     docker push <acr-name>.azurecr.io/<service-name>:<tag>  
     ```  

---

### **Step 4: Kubernetes Setup**  
1. **Connect to AKS Cluster**:  
   ```bash  
   az aks get-credentials --resource-group <resource-group-name> --name <aks-name>  
   ```  

2. **Create Namespaces**:  
   ```bash  
   kubectl create namespace default  
   kubectl create namespace devops  
   kubectl create namespace monitoring  
   kubectl create namespace vault  
   ```  

3. **Deploy Base Services**:  
   - Deploy MySQL, Redis, Kafka, and Zookeeper using Helm or Kubernetes manifests.  

4. **Deploy Microservices**:  
   - Create Helm charts for each service and deploy:  
     ```bash  
     helm install <release-name> ./helm-charts/<service-name> -n default  
     ```  

---

### **Step 5: CI/CD Pipeline Setup**  
1. **Set Up Jenkins**:  
   - Deploy Jenkins in the `devops` namespace:  
     ```bash  
     helm install jenkins ./helm-charts/jenkins -n devops  
     ```  
   - Create Jenkins pipelines for build and deployment stages.  

2. **Configure GitLab**:  
   - Push your application code to GitLab repositories.  
   - Integrate GitLab with Jenkins for pipeline triggers.  

3. **Install ArgoCD**:  
   - Deploy ArgoCD in the `devops` namespace:  
     ```bash  
     kubectl apply -n devops -f argocd-install.yaml  
     ```  
   - Use ArgoCD for GitOps-based continuous deployment.  

---

### **Step 6: Monitoring and Observability**  
1. **Deploy Prometheus and Grafana**:  
   - Install Prometheus and Grafana in the `monitoring` namespace using Helm:  
     ```bash  
     helm install prometheus ./helm-charts/prometheus -n monitoring  
     helm install grafana ./helm-charts/grafana -n monitoring  
     ```  

2. **Access Grafana Dashboards**:  
   - Use the Grafana web interface to monitor application metrics.  

---

### **Step 7: Secrets Management**  
1. **Install HashiCorp Vault**:  
   - Deploy Vault in the `vault` namespace using Helm:  
     ```bash  
     helm install vault ./helm-charts/vault -n vault  
     ```  
   - Configure Vault to manage sensitive secrets.  

---

### **Step 8: Secure Traffic with HTTPS**  
1. **Install cert-manager**:  
   ```bash  
   helm install cert-manager ./helm-charts/cert-manager -n devops  
   ```  

2. **Configure Ingress-NGINX**:  
   - Deploy Ingress-NGINX in the `default` namespace:  
     ```bash  
     helm install ingress-nginx ./helm-charts/ingress-nginx -n default  
     ```  
   - Set up ingress rules for HTTPS traffic routing.  

---

### **Step 9: Validation and Testing**  
1. **Validate Deployment**:  
   - Verify that all microservices are running in the Kubernetes cluster.  
   - Test the CI/CD pipelines by pushing updates to GitLab and ensuring automatic deployments.  

2. **Test Observability**:  
   - Check Grafana dashboards for application metrics and Prometheus alerts.  

3. **Access Application**:  
   - Use the Ingress URL to access the deployed application over HTTPS.  

---

### **Step 10: Celebrate Your Success** 🎉  
You did it! Your microservices application is live, scalable, monitored, and secure.  

---
