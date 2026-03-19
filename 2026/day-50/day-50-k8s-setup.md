# Day 50 – Kubernetes Architecture and Cluster Setup


## Kubernetes Story

Kubernetes was created to solve the challenges of managing containerized applications at scale. While Docker made it easy to run containers, managing hundreds of containers across multiple servers became complex.

Manual container management introduced several issues such as difficulty in scaling, lack of self-healing, complex networking, and no built-in load balancing.

Kubernetes automates deployment, scaling, networking, and management of containers, making it easier to handle large-scale applications.

Kubernetes was developed by Google and inspired by its internal system called Borg. It is written in Go language.

The word "Kubernetes" comes from Greek, meaning "Helmsman" or "Ship Captain", which represents controlling and managing containerized applications.


##  Kubernetes Architecture

Kubernetes architecture consists of two main components:

###  Control Plane

* **API Server**: Acts as the entry point for all commands and communication.
* **etcd**: A key-value database that stores the cluster state.
* **Scheduler**: Assigns pods to appropriate worker nodes.
* **Controller Manager**: Ensures the desired state matches the actual state.

###  Worker Node

* **kubelet**: Node agent that receives instructions and manages pods.
* **kube-proxy**: Handles networking and communication between pods.
* **Container Runtime**: Runs the actual containers (e.g., containerd).


##  kubectl apply Flow
When we run:

kubectl apply -f pod.yaml

The following happens:

1. The request is sent to the API Server
2. The API Server validates the request
3. The desired state is stored in etcd
4. The Scheduler assigns a node
5. The kubelet ensures the pod runs on that node
6. The container runtime starts the container


##  kubectl Installation

kubectl is a command-line tool used to interact with the Kubernetes cluster.

### Installation (Windows)

kubectl was already installed on the system.

### Verification

kubectl version --client

## Cluster Setup (kind)

For local Kubernetes cluster setup, **kind (Kubernetes in Docker)** was used.

### Why kind?

* Lightweight and fast
* Uses Docker containers as nodes
* Ideal for DevOps practice

### Create Cluster

kind create cluster --name devops-cluster

### Verify Cluster

kubectl cluster-info
kubectl get nodes

## Cluster Exploration

### List Namespaces

kubectl get namespaces

### List All Pods

kubectl get pods -A

### kube-system Pods

kubectl get pods -n kube-system
### kube-system Components

* **kube-apiserver**: Entry point of the cluster
* **etcd**: Stores cluster data
* **kube-scheduler**: Assigns pods to nodes
* **kube-controller-manager**: Maintains desired state
* **coredns**: Handles DNS and service discovery
* **kube-proxy**: Manages networking rules

##  Cluster Lifecycle

### Delete Cluster

kind delete cluster --name devops-cluster

### Recreate Cluster

kind create cluster --name devops-cluster

### Verify

kubectl get nodes

## kubeconfig

kubeconfig is a configuration file used by kubectl to connect to the Kubernetes cluster.

It stores:

* Cluster details
* User credentials
* Context information

### Default Location

~/.kube/config

On Windows:
C:\Users\<your-user>\.kube\config


