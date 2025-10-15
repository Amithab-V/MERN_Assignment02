Assignment - Container Orchestration

Assignment Objectives
1.	Kubernetes Deployment Files
o	Create separate deployment and service YAMLs for frontend and backend.
o	Ensure scalability using replicas and resource configuration.
o	Connect both components via internal networking (ClusterIP Services).
o	Deploy MongoDB as a StatefulSet for persistent data.
2.	Helm Chart
o	Package all Kubernetes manifests into a reusable Helm chart.
o	Use values.yaml to manage environment-specific configurations.
o	Include templates for:
	Frontend Deployment & Service
	Backend Deployment & Service
	MongoDB StatefulSet & Service
	ConfigMap and Secrets
	Optional Ingress
3.	Jenkins CI/CD Pipeline
o	Automate build and deployment using a Jenkinsfile.
o	Stages include:
	Code checkout (Frontend & Backend Repos)
	Docker image build
	Push to Docker registry
	Helm upgrade/install deployment
o	Use Jenkins credentials for Docker registry and Kubernetes access.

Project Structure

	k8s/
o	backend-deployment.yaml
o	backend-service.yaml
o	frontend-deployment.yaml
o	frontend-service.yaml
o	mongodb-statefulset.yaml
o	mongodb-service.yaml
	helm-chart/mern-chart/
	Chart.yaml
	values.yaml
	templates/
o	deployment-backend.yaml
o	configmap.yaml
o	service-mongodb.yaml
o	deployment-frontend.yaml
	Jenkinsfile
	Dockerfile.frontend
	Dockerfile.backend
README.md

	Before deploying, ensure the following tools are installed and configured:
•	Docker
•	Kubernetes (kubectl)
•	Helm v3+
•	Jenkins
•	Access to a Docker registry
•	GitHub repositories for frontend and backend codebases

	Clone the Repositories
	git clone https://github.com/your-org/mern-frontend.git frontend
	git clone https://github.com/your-org/mern-backend.git backend

	Build Docker Images
	docker build -t your-registry/mern-frontend:latest -f Dockerfile.frontend ./frontend
	docker build -t your-registry/mern-backend:latest -f Dockerfile.backend ./backend
	docker push your-registry/mern-frontend:latest
	docker push your-registry/mern-backend:latest

	Deploy using Helm
	helm install mern-app helm-chart/mern-chart --namespace mern --create-namespace



	helm upgrade mern-app helm-chart/mern-chart

	helm upgrade mern-app helm-chart/mern-chart


The Jenkinsfile automates:
1.	Checkout – Pulls frontend and backend repositories.
2.	Build – Installs dependencies and builds Docker images.
3.	Push – Pushes images to Docker registry.
4.	Deploy – Deploys via Helm chart to Kubernetes.


Pipeline Configuration
•	Credentials:
o	DOCKERCREDS → Docker registry (username/password)
o	KUBECONFIG → Kubernetes cluster access file
•	Tools required: docker, kubectl, helm, and node.
Run pipeline in Jenkins → it will automatically:
•	Build Docker images
•	Push to your registry
•	Deploy to your Kubernetes cluster

	Check deployment status:
	kubectl get pods -n mern
	kubectl get svc -n mern


	Access the frontend
	kubectl get svc mern-frontend-svc -n mern

	Port-forward
	kubectl port-forward svc/mern-frontend-svc 8080:80 -n mern
