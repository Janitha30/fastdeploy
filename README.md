# 🚀 FastDeploy – End-to-End DevOps CI/CD Pipeline

FastDeploy is a hands-on DevOps project that automates application testing, Docker image building, and Kubernetes deployment using Jenkins. It uses FastAPI for the application and Prometheus with Grafana for monitoring.

## 🏗️ Architecture

```text
Developer
   |
   v
GitHub Repository
   |
   v
Jenkins CI/CD Pipeline
   |
   +--> Run Tests (Pytest)
   |
   +--> Build Docker Image
   |
   +--> Load Image into Minikube
   |
   +--> Deploy to Kubernetes
   |
   +--> Verify Deployment
             |
             v
       FastAPI Application
             |
             v
    Prometheus + Grafana
       (Monitoring)
```

## 🛠️ Technologies Used

- **Cloud:** AWS EC2
- **Operating System:** Ubuntu Linux
- **CI/CD:** Jenkins
- **Containerization:** Docker
- **Orchestration:** Kubernetes, Minikube, kubectl
- **Application:** Python, FastAPI
- **Testing:** Pytest
- **Monitoring:** Prometheus, Grafana, kube-prometheus-stack
- **Version Control:** Git, GitHub
- **Package Management:** Helm

## ✨ Key Features

- Automated CI/CD pipeline using Jenkins.
- Automated application testing with Pytest.
- Docker image build and deployment workflow.
- Kubernetes Deployment with two replicas.
- Readiness and liveness probes for application health checks.
- Kubernetes Service using NodePort.
- Prometheus-based infrastructure and Kubernetes monitoring.
- Grafana dashboards for metrics visualization.
- Deployment rollout verification through Jenkins.

## 📂 Project Structure

```text
fastdeploy/
├── app/                 # FastAPI application (if applicable)
├── tests/               # Application tests
├── k8s/
│   └── deployment.yaml  # Kubernetes deployment configuration
├── Dockerfile           # Docker image configuration
├── Jenkinsfile          # CI/CD pipeline
├── requirements.txt     # Python dependencies
└── README.md            # Project documentation
```

*Update the folder structure above to match your actual repository.*

## ⚙️ CI/CD Pipeline Stages

1. **Test:** Run automated tests to validate application functionality.
2. **Build Docker Image:** Create a Docker image for the FastAPI application.
3. **Load Image into Minikube:** Make the built image available to the local Kubernetes cluster.
4. **Deploy to Kubernetes:** Deploy the application and update the image version.
5. **Verify Deployment:** Confirm that the Kubernetes rollout completes successfully.

## ☸️ Kubernetes Deployment

The application is deployed with two replicas and configured with readiness and liveness probes.

Check deployment status:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
kubectl rollout status deployment/fastdeploy
```

## 📊 Monitoring with Prometheus and Grafana

The project uses the Prometheus and Grafana monitoring stack installed through Helm.

Monitoring components include:

- Prometheus for metrics collection.
- Grafana for dashboards and visualization.
- Node Exporter for node-level metrics.
- Kube-state-metrics for Kubernetes resource-state metrics.
- Alertmanager for alert management.

Check monitoring components:

```bash
kubectl get pods -n monitoring
kubectl get services -n monitoring
```

For secure local Grafana access, use port forwarding:

```bash
kubectl port-forward -n monitoring svc/monitoring-grafana 3001:80
```

Then use an SSH tunnel from your local machine to access Grafana at `http://localhost:3000`.

## 🚀 Application Endpoint

The FastAPI application returns:

```json
{
  "message": "FastDeploy is running"
}
```

## 🎯 Learning Outcomes

- Built a Jenkins CI/CD pipeline from source control to Kubernetes deployment.
- Practiced Docker image creation and container workflows.
- Deployed and verified a multi-replica Kubernetes application.
- Configured application health probes and Kubernetes Services.
- Set up Prometheus and Grafana for monitoring a Kubernetes environment.
- Practiced Linux administration and troubleshooting on AWS EC2.

## 🔮 Future Improvements

- Add automated deployment triggers through GitHub webhooks.
- Publish Docker images to Amazon ECR.
- Configure CI/CD notifications.
- Create custom Grafana dashboards and alert rules.
- Add infrastructure provisioning using Terraform.
- Introduce security scanning for application dependencies and Docker images.

## 👨‍💻 Author

**Janitha M J**

DevOps | Linux | AWS | Docker | Kubernetes | Jenkins | Prometheus | Grafana

GitHub: [Janitha30](https://github.com/Janitha30)
