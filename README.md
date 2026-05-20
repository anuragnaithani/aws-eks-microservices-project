# AWS EKS Three-Tier E-Commerce Microservices Project

## Overview

This project demonstrates deployment of a production-grade three-tier e-commerce application on AWS EKS using Kubernetes, Helm, AWS Load Balancer Controller, and EBS CSI Driver.

The application follows a microservices architecture where frontend, backend services, databases, and messaging systems run inside Kubernetes pods on Amazon EKS.

---

# Project Architecture

```text
User / Browser
      ↓
AWS Application Load Balancer
      ↓
Kubernetes Ingress
      ↓
Web Frontend
      ↓
Backend Microservices
      ↓
MongoDB / MySQL / Redis / RabbitMQ
      ↓
Amazon EKS Cluster with EC2 Worker Nodes
```

---

# Three-Tier Architecture

## 1. Presentation Layer

The frontend UI is served using Nginx and AngularJS.

Responsibilities:

- User interaction
- Product display
- Cart access
- Checkout interface

---

## 2. Application Layer

Backend microservices handle business logic.

Services:

- Cart
- Catalogue
- User
- Payment
- Shipping
- Ratings
- Dispatch

---

## 3. Data Layer

Databases and supporting services store and process application data.

Components:

- MongoDB
- MySQL
- Redis
- RabbitMQ

---
# Why Amazon EKS?

Amazon EKS (Elastic Kubernetes Service) is a managed Kubernetes service provided by AWS.

It helps deploy, manage, and scale containerized applications without manually managing Kubernetes control plane components.

Benefits of EKS:

- Managed Kubernetes cluster
- High availability
- Automatic scaling support
- AWS integration
- Secure IAM authentication
- Production-grade orchestration

---

# Why Kubernetes?

Kubernetes is used to manage containerized microservices.

In this project Kubernetes helps with:

- Pod management
- Auto-healing
- Service discovery
- Load balancing
- Scaling
- Rolling updates

Each microservice runs inside its own Kubernetes pod.

---

# Why Helm?

Helm is a package manager for Kubernetes.

Instead of manually creating multiple YAML files, Helm helps deploy applications quickly using charts.

In this project Helm was used to:

- Deploy Robot Shop application
- Manage Kubernetes resources
- Simplify deployment process

---

# Why AWS Load Balancer Controller?

AWS Load Balancer Controller automatically creates AWS Application Load Balancers for Kubernetes Ingress resources.

Benefits:

- Public access to application
- Path-based routing
- Integration with Kubernetes Ingress
- Automatic ALB provisioning

Without this controller, Kubernetes Ingress would not create AWS ALB automatically.

---

# Why EBS CSI Driver?

EBS CSI Driver allows Kubernetes pods to use Amazon EBS volumes as persistent storage.

This is important because databases need persistent data storage.

Used for:

- MongoDB storage
- MySQL storage
- Persistent volumes

---

# Microservices Communication

All services communicate internally using Kubernetes services.

Example:

```text
Web → Catalogue → MongoDB
Web → User → Redis
Web → Payment → RabbitMQ
```

Kubernetes DNS automatically handles service discovery.

---

# Networking Flow

```text
User Request
     ↓
AWS Application Load Balancer
     ↓
Ingress Controller
     ↓
Kubernetes Service
     ↓
Kubernetes Pod
```

This flow allows external users to securely access applications running inside Kubernetes.

---

# Kubernetes Resources Used

| Resource | Purpose |
|---|---|
| Pod | Runs container |
| Deployment | Manages replicas |
| Service | Internal communication |
| Ingress | External access |
| Namespace | Resource isolation |
| Persistent Volume | Storage |
| ConfigMap | Configuration |
| Secret | Sensitive data |

---

# Real-World DevOps Concepts Used

This project demonstrates:

- Container orchestration
- Infrastructure automation
- Cloud-native deployment
- Kubernetes networking
- Persistent storage
- Ingress management
- Microservices architecture
- AWS cloud integration

---

# Challenges Faced During Deployment

## 1. CloudFormation Rollback

Initially cluster creation failed due to AWS service temporary errors.

Solution:
- Deleted failed stacks
- Recreated cluster

---

## 2. ALB Controller IAM Issues

IAM permissions were required for ALB Controller.

Solution:
- Created IAM OIDC provider
- Attached IAM policies correctly

---

## 3. EBS CSI Driver Configuration

Persistent storage setup required CSI driver integration.

Solution:
- Installed EBS CSI Driver addon
- Configured IAM role

---

# Key Learning Outcomes

After completing this project I learned:

- Production-style EKS deployment
- Kubernetes architecture
- AWS networking basics
- Helm deployment strategy
- Kubernetes Ingress
- Persistent storage management
- Microservices deployment
- Cloud cost cleanup practices
---

# AWS Services Used

| AWS Service | Purpose |
|---|---|
| Amazon EKS | Kubernetes cluster |
| EC2 | Worker nodes |
| ALB | Public traffic |
| IAM OIDC | IAM integration |
| EBS CSI Driver | Persistent storage |
| CloudFormation | Infrastructure |
| VPC | Networking |

---

# Tools Used

- AWS CLI
- eksctl
- kubectl
- Helm
- Docker
- Kubernetes
- GitHub

---

# Deployment Steps

## Clone Repository

```bash
git clone https://github.com/anuragnaithani/aws-eks-microservices-project.git
cd aws-eks-microservices-project/EKS
```

---

## Create EKS Cluster

```bash
eksctl create cluster \
  --name robot-shop-cluster \
  --region us-west-2 \
  --nodegroup-name robot-shop-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --managed
```

---

## Configure kubectl

```bash
aws eks update-kubeconfig \
  --region us-west-2 \
  --name robot-shop-cluster
```

---

## Install AWS Load Balancer Controller

```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update
```

---

## Deploy Application

```bash
kubectl create namespace robot-shop

cd helm

helm install robot-shop . -n robot-shop
```

---

# Deployment Proof

## EKS Cluster

![EKS Cluster](screenshots/01-aws-eks-cluster-active.png)

---

## Pods Running

![Pods Running](screenshots/04-robot-shop-pods-running.png)

---

## Ingress ALB

![Ingress](screenshots/06-robot-shop-ingress-alb.png)

---

## Application UI

![Homepage](screenshots/07-robot-shop-homepage.png)

---

## AWS Load Balancer

![Load Balancer](screenshots/11-aws-application-load-balancer.png)

---

# Important Commands

```bash
kubectl get nodes

kubectl get pods -n robot-shop

kubectl get svc -n robot-shop

kubectl get ingress -n robot-shop
```

---

# Cleanup

```bash
eksctl delete cluster \
  --region us-west-2 \
  --name robot-shop-cluster
```

---

# What I Learned

- AWS EKS deployment
- Kubernetes microservices
- Helm deployment
- AWS ALB Ingress
- EBS CSI Driver
- Kubernetes networking
- Real-world DevOps workflow
- AWS cleanup and cost management

---

# Project Status

Completed Successfully ✅
