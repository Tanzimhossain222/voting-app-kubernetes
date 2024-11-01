
# Voting App on Kubernetes

This document provides a comprehensive guide for deploying a simple voting application using Kubernetes. The application consists of two primary services: a **Voting Service** that allows users to vote between two options and a **Result Service** that displays the aggregated results of the votes.

## Architecture

![Voting App Architecture](./images/voting-app-architecture.png)

## Prerequisites

Before you begin, ensure that you have the following installed on your local machine:

- **[Docker](https://docs.docker.com/get-docker/)**: A platform for developing, shipping, and running applications inside containers.
- **[Kubernetes](https://kubernetes.io/docs/setup/)**: An open-source system for automating deployment, scaling, and management of containerized applications.
- **[Minikube](https://minikube.sigs.k8s.io/docs/start/)** (optional but recommended for local development): A tool that makes it easy to run Kubernetes locally.

## Application Overview

The application utilizes Docker images available at the [example-voting-app GitHub repository](https://github.com/dockersamples/example-voting-app). This repository contains pre-built images for all the necessary components of the application.

## Running the Application

Follow these steps to deploy the voting application on your Kubernetes cluster:

### Step 1: Check Existing Pods and Services

Before deploying the application, check the current status of any running deploy and services in your Kubernetes cluster:

```bash
kubectl get deploy,svc
```

### Step 2: Deploy the Voting Service

Deploy the voting service using the deployment and service YAML files:

```bash
kubectl apply -f voting-app/deploy/voting-app-deploy.yaml
kubectl apply -f voting-app/service/voting-app-service.yaml
```

### Step 3: Deploy Redis

Redis is used for storing votes temporarily. Deploy the Redis service and deployment:

```bash
kubectl apply -f redis/deploy/redis-deploy.yaml
kubectl apply -f redis/service/redis-service.yaml
```

### Step 4: Deploy PostgreSQL

PostgreSQL is used as the database to store the final voting results. Deploy the PostgreSQL service and deployment:

```bash
kubectl apply -f postgres/deploy/postgres-deploy.yaml
kubectl apply -f postgres/service/postgres-service.yaml
```

### Step 5: Deploy the Worker Service

The Worker service processes votes and stores them in the database. Deploy the worker service:

```bash
kubectl apply -f worker-app/deploy/worker-app-deploy.yaml
```

### Step 6: Deploy the Result Service

Finally, deploy the result service to display the voting results:

```bash
kubectl apply -f result-app/deploy/result-app-deploy.yaml
kubectl apply -f result-app/service/result-app-service.yaml
```

### Step 7: Check the Status of Pods and Services

After deploying all components, check the status of the deploy and services to ensure they are running correctly:

```bash
kubectl get deploy,svc
```

### Step 8: Access the Voting Application

To open the voting app in your web browser, run the following command, which provides the service URL:

```bash
minikube service voting-service --url
```

### Step 9: Access the Result Application

Similarly, to view the results of the voting, open the result app by running:

```bash
minikube service result-service  --url
```

### Step 10: Scale up Deployments (Optional)

```bash
kubectl scale deployment voting-app-deploy  --replicas=3
kubectl scale deployment result-app-deployment --replicas=3
kubectl scale deployment worker-app-deployment --replicas=3
```


## Conclusion

Congratulations! You have successfully deployed the voting application on your Kubernetes cluster. You can now use the voting service to cast votes and view the results in real-time. If you encounter any issues, ensure that all pods and services are running correctly by checking their status with `kubectl get pods,svc`.

Feel free to customize and extend this application as needed. Happy voting!

