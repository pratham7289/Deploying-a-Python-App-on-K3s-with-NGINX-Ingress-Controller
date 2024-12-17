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
