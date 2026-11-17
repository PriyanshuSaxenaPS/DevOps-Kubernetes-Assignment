# Kubernetes Full Stack Application

## Project Description

This repository contains my completed practical assignment for the **TuteDude DevOps** module. This project demonstrates deploying a containerized full-stack application using **Flask**, **Express.js**, **Docker**, **Kubernetes**, and **Minikube**.

---

## Technologies Used

- Python
- Flask (with Gunicorn & Requests)
- Node.js
- Express.js
- Docker
- Kubernetes
- Minikube
- kubectl

---

## Application Architecture

The application consists of four core components:

1. **Flask Frontend** — Serves the web UI and health endpoint (`/health`) on port `5000` (2 pod replicas).
2. **Express Backend** — Serves the REST API (`/api/message`) on port `5001` (2 pod replicas).
3. **Kubernetes Deployments** — Manages pod replicas and container images (`flask-frontend:v1` and `express-backend:v1`).
4. **Kubernetes Services** — Handles internal service discovery and external traffic routing.

---

## Service Ports & Kubernetes Services

* **Frontend (`frontend-service`):** Flask frontend runs on container port `5000`, exposed externally via **NodePort `30080`**.
* **Backend (`backend-service`):** Express backend runs on container port `5001`, exposed internally via **ClusterIP `5001`**.

---

## Deployment

### 1. Clone the Repository
```bash
git clone https://github.com
cd Kubernetes-Assignment
```

### 2. Start Minikube
```bash
minikube start --driver=docker
minikube status
```

### 3. Build Images Inside Minikube
```bash
minikube image build -t flask-frontend:v1 ./frontend
minikube image build -t express-backend:v1 ./backend
minikube image ls
```

### 4. Apply Kubernetes Files
```bash
kubectl apply -f kubernetes/
```

### 5. Check Pods & Services
```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

### 6. Access Frontend
* **Using Minikube Service Tunnel:**
  ```bash
  minikube service frontend-service
  ```
* **Using `kubectl` Port-Forwarding:**
  ```bash
  kubectl port-forward service/frontend-service 8080:5000
  ```
  Open [http://localhost:8080](http://localhost:8080) in your browser to view the application.

---

## Repository Structure

```text
Kubernetes-Assignment/
├── backend/
│   ├── Dockerfile                  # Express backend container build instructions
│   ├── package.json                # Node.js dependencies
│   └── server.js                   # Express API serving /api/message on port 5001
├── frontend/
│   ├── app.py                      # Flask frontend UI & /health endpoint on port 5000
│   ├── Dockerfile                  # Flask frontend container build instructions
│   └── requirements.txt            # Python dependencies
├── kubernetes/
│   ├── backend-deployment.yaml     # Express backend Deployment (2 replicas)
│   ├── backend-service.yaml        # ClusterIP Service exposing port 5001
│   ├── frontend-deployment.yaml    # Flask frontend Deployment (2 replicas)
│   └── frontend-service.yaml       # NodePort Service exposing port 5000 -> 30080
├── .gitignore                      # Excludes local environment and cache files
└── README.md                       # Project documentation
```
