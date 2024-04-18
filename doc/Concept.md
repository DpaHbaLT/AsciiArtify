# Introduction

This document presents a comparative analysis of three tools for deploying local Kubernetes clusters: Minikube, kind, and k3d. Each tool has its own advantages and disadvantages, which should be taken into account when choosing for the "AsciiArtify" startup's Proof of Concept (PoC).

# Characteristics

|   |Minikube|kind (Kubernetes IN Docker)|k3d|
| ------------ | ------------ | ------------ | ------------ |
|Purpose|  Local Kubernetes cluster on a single computer.|Creating local Kubernetes clusters in Docker containers.|Local Kubernetes clusters in Docker containers using RKE.|
|Supported OS and architectures|Windows, macOS, Linux.|Windows, macOS, Linux.|Windows, macOS, Linux.|
|Automation capabilities|Support for bash scripts and other automation tools.|Easily automated through Docker and scripts.|Fast cluster creation and deletion, support for network policies.|
|Additional features|Additional configuration options via command-line options.|Easily automated through Docker and scripts.|Helm support for package management.|

# Advantages and Disadvantages

|   |Advantages|Disadvantages|
| ------------ | ------------ | ------------ |
|Minikube|Ease of use for beginners. Quick deployment of a local cluster.|Limited scalability. Stability issues on some platforms.|
|kind|High flexibility and customization. Good community support for Kubernetes.|Requires Docker to function, which may pose licensing issues.|
|k3d|Fast cluster creation and testing. Helm support for package management.|Requires Docker to function, which may pose licensing issues.|

# Demonstration

Demonstration of using k3d to deploy a "Hello World" application on Kubernetes:

[![asciicast](https://asciinema.org/a/lE8AadUJuOpKDovLhnLkYLPl2.svg)](https://asciinema.org/a/lE8AadUJuOpKDovLhnLkYLPl2)

```bash
# Create a kind Kubernetes cluster
k3d cluster create demo

# Check cluster status
kubectl cluster-info

# Deploy the "Hello World" application
kubectl create deployment hello-world --image=gcr.io/google-samples/hello-app:1.0

# Expose the application to external access
kubectl expose deployment hello-world --type=NodePort --port=8080

# Check access to the application
kubectl get service hello-world

# Forward the port to localhost
kubectl port-forward svc/hello-world 8080:8080

```

#Conclusions

Considering the needs of the "AsciiArtify" startup and the features of each tool, it is recommended to use k3d for the PoC. k3d provides flexibility and ease of use, has an active community, and broad Kubernetes support. Additionally, considering potential issues with Docker licensing, an alternative option could be Podman instead of Docker. Podman offers similar functionality but does not have licensing restrictions, which may reduce risks for the startup.