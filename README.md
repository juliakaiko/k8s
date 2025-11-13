# Microservices Application on Kubernetes

A containerized microservices application deployed on Kubernetes with Spring Boot services, PostgreSQL, MongoDB, Redis, and Kafka.

## Architecture Overview

- **API Gateway**: Spring Cloud Gateway (port 8080)
- **Authentication Service**: JWT-based auth (port 8081)
- **User Service**: User management (port 8083)
- **Order Service**: Order processing with Kafka (port 8082)
- **Payment Service**: Payment processing with MongoDB (port 8084)

## Prerequisites

- Kubernetes cluster (Minikube, Docker Desktop, or cloud provider)
- kubectl configured
- Docker for building images

## Quick Start

### 1. Build Docker Images
- docker build -t authservice:latest ./auth-service
- docker build -t userservice:latest ./user-service
- docker build -t orderservice:latest ./order-service
- docker build -t paymentservice:latest ./payment-service
- docker build -t gateway:latest ./gateway

### 2. Deploy to Kubernetes
- kubectl apply -f k8s/01-configs/
- kubectl apply -f k8s/02-infrastructure/
- kubectl apply -f k8s/03-services/
- kubectl apply -f k8s/04-networking/

### 3. Access the Application
Add to your C:\Windows\System32\drivers\etc\hosts: **127.0.0.1 innowise-project.local**

## Monitoring
**All services include:**
- Liveness probes
- Readiness probes
- Startup probes
- Resource limits (CPU/Memory)

## Development
- Uses imagePullPolicy: Never for local development in Docker
- Init containers ensure dependency readiness
- ConfigMap and Secrets for configuration management
- RBAC for service discovery in gateway

## Testing
- kubectl get pods
- kubectl logs <pod-name>
- kubectl describe pod <pod-name>

## Launching the app

- minikube start --driver=docker

- Configure Docker to use Minikube’s Docker daemon
  - minikube docker-env | Invoke-Expression

- Make sure that Docker is actually connected to Minikube:
  - docker info | findstr "Name"

- In a separate terminal window:
  - minikube tunnel

- cd ... (specify the project root here, for example: "cd D:\Julia\INNOWICE\rootproject")

- Build services in Docker:
  - docker build -t gateway:latest ./gateway
  - docker build -t authservice:latest ./authservice
  - docker build -t userservice:latest ./userservice
  - docker build -t orderservice:latest ./orderservice
  - docker build -t paymentservice:latest ./paymentservice

- Apply the manifests by folder (in the correct order):
  - kubectl apply -f k8s/01-configs/
  - kubectl apply -f k8s/02-infrastructure/
  - kubectl apply -f k8s/03-services/
  - kubectl apply -f k8s/04-networking/

- Install the official manifest for NGINX Ingress Controller deployment in Kubernetes:
  - kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml

- Check that the controller is running:
  - kubectl get pods -n ingress-nginx
  - minikube service ingress-nginx-controller -n ingress-nginx --url

- Check all pods readiness "1/1"
  - kubectl get pods -w
  
- minikube stop