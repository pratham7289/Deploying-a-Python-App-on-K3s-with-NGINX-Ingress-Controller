# Deploying-a-Python-App-on-K3s-with-NGINX-Ingress-Controller
This guide provides step-by-step instructions to deploy a Python Flask application in a K3s Kubernetes cluster, expose it using a Service, and route external traffic using the NGINX Ingress Controller.

Table of Contents
Prerequisites
K3s Installation
Build and Push Docker Image
Deploy the Application
Configure DNS
Verify Deployment
Why This Setup?
Project Structure
1. Prerequisites
Ensure the following tools are installed:

K3s (lightweight Kubernetes distribution)
Docker (for building images)
kubectl (Kubernetes CLI)
A Virtual Machine (Ubuntu Server) or any environment with K3s set up.
2. K3s Installation
Install K3s using the official installation script:

bash
Copy code
curl -sfL https://get.k3s.io | sh -
Verify that the K3s cluster is up and running:

bash
Copy code
sudo k3s kubectl get nodes
3. Build and Push Docker Image
Create a Dockerfile to containerize the Python application.
Build the Docker image:
bash
Copy code
docker build -t my-python-app:latest .
Tag and push the image to Docker Hub or any container registry:
bash
Copy code
docker tag my-python-app:latest <dockerhub-username>/my-python-app:latest
docker push <dockerhub-username>/my-python-app:latest
4. Deploy the Application
Deploy the Python application in the K3s cluster with the following components:

Deployment: Ensures the application is running in multiple replicas for reliability.
Service: Provides internal networking to expose the app to the cluster.
Ingress: Routes external traffic to the service using a clean domain name.
Apply these manifests using kubectl:

bash
Copy code
kubectl apply -f <deployment-file>
kubectl apply -f <service-file>
kubectl apply -f <ingress-file>
5. Configure DNS
Map the domain my-python-app.local to the K3s node's IP:

Get the Node IP:
bash
Copy code
kubectl get nodes -o wide
Update the /etc/hosts file on your local machine:
plaintext
Copy code
<Node-IP> my-python-app.local
Replace <Node-IP> with the actual node IP.

6. Verify Deployment
Access the application via a browser or curl:

bash
Copy code
curl http://my-python-app.local
You should see the expected output from your Python application.

7. Why This Setup?
Component	Purpose
K3s	Lightweight Kubernetes cluster for deploying containerized applications.
NGINX Ingress	Manages external access to services running inside the cluster.
Docker Image	Containerizes the Python app for portability and scalability.
Deployment	Ensures reliable and scalable application deployment.
Service	Provides internal cluster networking for the app.
Ingress	Routes traffic to the Python app using a clean domain name.
DNS Mapping	Allows local testing using a custom domain (my-python-app.local).
8. Project Structure
plaintext
Copy code
.
├── Dockerfile
├── app.py
├── requirements.txt
├── deployment.yaml
├── service.yaml
└── ingress.yaml
Next Steps
Customize the Python app (app.py) for additional features.
Add monitoring using Prometheus and Grafana.
Explore Persistent Storage and ConfigMaps for managing configurations
