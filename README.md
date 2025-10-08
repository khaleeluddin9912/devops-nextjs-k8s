# Next.js App Kubernetes Deployment

This repository contains a **Next.js application** containerized with Docker, built using **GitHub Actions**, pushed to **GitHub Container Registry (GHCR)**, and deployed on a **Kubernetes cluster** using Minikube.

---

## Project Overview

This project demonstrates the full **DevOps workflow**:

1. Build a Next.js application.
2. Containerize it with Docker.
3. Automate Docker build & push to GHCR using GitHub Actions.
4. Deploy the app on Kubernetes using Minikube-ready manifests.

---

## Step 1 – Run Next.js Locally

Clone the repository:


git clone https://github.com/khaleeluddin9912/devops-nextjs-k8s.git
cd my-next-app
Install dependencies:

bash
Copy code
npm install
Run the app locally:

bash
Copy code
npm run dev
The app will be available at: http://localhost:3000

---

## Step 2 – Docker Build and Run
Build Docker image:

bash
Copy code
docker build -t nextjs-app .
Run Docker container:

bash
Copy code
docker run -p 3000:3000 nextjs-app
The app is now running in a container on port 3000.

---

## Step 3 – GitHub Actions Workflow
The workflow automatically:

Builds the Docker image.

Pushes it to GitHub Container Registry (GHCR).

Workflow file: .github/workflows/docker-build.yml

Docker image pushed to: ghcr.io/khaleeluddin9912/nextjs-app:latest

---

## Step 4 – Kubernetes Deployment
Deployment Manifest (k8s/deployment.yaml)
yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nextjs-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nextjs-app
  template:
    metadata:
      labels:
        app: nextjs-app
    spec:
      containers:
      - name: nextjs
        image: ghcr.io/khaleeluddin9912/nextjs-app:latest
        ports:
        - containerPort: 3000
Service Manifest (k8s/service.yaml)
yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: nextjs-service
spec:
  type: NodePort
  selector:
    app: nextjs-app
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 30080

---

## Step 5 – Deploy on Minikube
Start Minikube:

bash
Copy code
minikube start
Apply Kubernetes manifests:

bash
Copy code
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
Verify pods and service:

bash
Copy code
kubectl get pods
kubectl get svc
Access the app:

bash
Copy code
minikube service nextjs-service
The Next.js app should be running successfully on Kubernetes with 2 replicas.

Conclusion
This project demonstrates a complete DevOps workflow:

Next.js development

Docker containerization

CI/CD using GitHub Actions

Deployment on Kubernetes (Minikube)

The application is production-ready and follows modern DevOps practices.

yaml
Copy code

