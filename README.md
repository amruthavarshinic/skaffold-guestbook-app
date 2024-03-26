# skaffold-guestbook
build and deploy guestbook application in kubernetes with skaffold

## pre-requisites :

- kubernetes(kind)
- skaffold
- kompose
- guestbook application docker-compose.yaml

### Kubernetes (KIND):

**Kubernetes :** Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.(https://kubernetes.io/)

**Kind :**  kind is a tool for running local Kubernetes clusters using Docker container “nodes”. kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.(https://kind.sigs.k8s.io/)

```bash
# Install KIND
sudo curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
sudo chmod +x ./kind

# create a kind cluster
kind create cluster --name=sft-dev 
```

### Skaffold 

**Skaffold :** Skaffold is a command line tool that facilitates continuous development for Kubernetes-native applications. Skaffold handles the workflow for building, pushing, and deploying your application, and provides building blocks for creating CI/CD pipelines. This enables you to focus on iterating on your application locally while Skaffold continuously deploys to your local or remote Kubernetes cluster.(https://skaffold.dev/)

```bash
sudo curl -o /usr/local/bin/skaffold -L https://storage.googleapis.com/skaffold/releases/v1.26.1/skaffold-linux-amd64
sudo chmod +x /usr/local/bin/skaffold
```
### Kompose :

**Kompose :** Kompose is a conversion tool for Docker Compose to container orchestrators such as Kubernetes (or OpenShift).(https://kompose.io/)

```bash
wget https://github.com/kubernetes/kompose/releases/download/v1.26.1/kompose_1.26.1_amd64.deb # Replace 1.26.1 with latest tag
sudo apt install ./kompose_1.26.1_amd64.deb
```

### Gestbook docker-compose.yaml

```bash
version: "2"

services:

  redis-master:
    image: k8s.gcr.io/redis:e2e 
    ports:
      - "6379"

  redis-slave:
    image: gcr.io/google_samples/gb-redisslave:v1
    ports:
      - "6379"
    environment:
      - GET_HOSTS_FROM=dns

  frontend:
    image: gcr.io/google-samples/gb-frontend:v4
    ports:
      - "80:80"
    environment:
      - GET_HOSTS_FROM=dns
    labels:
      kompose.service.type: LoadBalancer
```

