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
docker build -t authservice:latest ./auth-service
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
- Resource limits (CPU/Me

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

- minikube start
- minikube tunnel (in a separate terminal window!)
- curl http://innowise-project.local/actuator/health 
- minikube stop