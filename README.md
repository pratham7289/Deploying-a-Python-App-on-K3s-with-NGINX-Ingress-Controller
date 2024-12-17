
# Deploying a Python Application on K3s with NGINX Ingress Controller

This guide provides step-by-step instructions to deploy a Python Flask application in a **K3s Kubernetes cluster**, expose it using a **Service**, and route external traffic using the **NGINX Ingress Controller**.

---

## Table of Contents
1. [Prerequisites](#prerequisites)  
2. [K3s Installation](#k3s-installation)  
3. [Build and Push Docker Image](#build-and-push-docker-image)  
4. [Deploy the Application](#deploy-the-application)  
5. [Configure DNS](#configure-dns)  
6. [Verify Deployment](#verify-deployment)  
7. [Why This Setup?](#why-this-setup)  
8. [Project Structure](#project-structure)  

---

## 1. Prerequisites

Ensure the following tools are installed:
- **K3s** (lightweight Kubernetes distribution)
- **Docker** (for building images)
- **kubectl** (Kubernetes CLI)
- A Virtual Machine (Ubuntu Server) or any environment with K3s set up.

---

## 2. K3s Installation

Install K3s using the official installation script:

```bash
curl -sfL https://get.k3s.io | sh -
```

Verify that the K3s cluster is up and running:

```bash
sudo k3s kubectl get nodes
```

---

## 3. Build and Push Docker Image

1. Create a **Dockerfile** to containerize the Python application.

   **Dockerfile**:

   ```dockerfile
   FROM python:3.9-slim

   WORKDIR /app

   COPY app.py requirements.txt ./

   RUN pip install --no-cache-dir -r requirements.txt

   EXPOSE 5000

   CMD ["python", "app.py"]
   ```

2. Build the Docker image:

   ```bash
   docker build -t my-python-app:latest .
   ```

3. Tag and push the image to Docker Hub or any container registry:

   ```bash
   docker tag my-python-app:latest <dockerhub-username>/my-python-app:latest
   docker push <dockerhub-username>/my-python-app:latest
   ```

---

## 4. Deploy the Application

Deploy the Python application in the K3s cluster with the following components:

1. **Deployment**: Ensures the application is running in multiple replicas for reliability.
2. **Service**: Provides internal networking to expose the app to the cluster.
3. **Ingress**: Routes external traffic to the service using a clean domain name.

Apply the manifests using `kubectl`:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

---

## 5. Configure DNS

Map the domain `my-python-app.local` to the K3s node's IP:

1. Get the Node IP:

   ```bash
   kubectl get nodes -o wide
   ```

2. Update the `/etc/hosts` file on your local machine:

   ```plaintext
   <Node-IP> my-python-app.local
   ```

Replace `<Node-IP>` with the actual node IP.

---

## 6. Verify Deployment

Access the application via a browser or `curl`:

```bash
curl http://my-python-app.local
```

You should see the expected output from your Python application.

---

## 7. Why This Setup?

| **Component**      | **Purpose**                                                                 |
|---------------------|----------------------------------------------------------------------------|
| **K3s**            | Lightweight Kubernetes cluster for deploying containerized applications.   |
| **NGINX Ingress**  | Manages external access to services running inside the cluster.            |
| **Docker Image**   | Containerizes the Python app for portability and scalability.              |
| **Deployment**     | Ensures reliable and scalable application deployment.                      |
| **Service**        | Provides internal cluster networking for the app.                          |
| **Ingress**        | Routes traffic to the Python app using a clean domain name.                |
| **DNS Mapping**    | Allows local testing using a custom domain (`my-python-app.local`).        |

---

## 8. Project Structure

```plaintext
.
├── Dockerfile
├── app.py
├── requirements.txt
├── deployment.yaml
├── service.yaml
└── ingress.yaml
```

---

## Next Steps

- Customize the Python app (`app.py`) for additional features.
- Add **monitoring** using Prometheus and Grafana.
- Explore **Persistent Storage** and **ConfigMaps** for managing configurations.

---

## License

This project is licensed under the MIT License.

---

## Contributions

Feel free to submit issues or pull requests to improve this project.
