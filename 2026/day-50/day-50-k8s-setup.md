# Task 1 – Kubernetes Story

1:- Why Kubernetes was created..?
 Kubernetes was created to manage containerized applications at scale. While Docker made it easy to run containers, managing hundreds of containers across multiple servers became complex.
Kubernetes provides:
Automated scaling
Self-healing
Load balancing
Automated deployment

2:- Who created Kubernetes..?
Kubernetes was developed by Google and inspired by its internal system called Borg. It is written in Go.

3:- Meaning of Kubernetes
Kubernetes means "Helmsman" or "Ship Captain", representing control over containerized applications.

# Task 2 – Kubernetes Architecture
Kubernetes Architecture 

Kubernetes architecture consists of:

Control Plane:

API Server (entry point)

etcd (database)

Scheduler (assigns pods to nodes)

Controller Manager (maintains desired state)

Worker Node:

kubelet (node agent)

kube-proxy (networking)

container runtime (runs containers)

# kubectl apply Flow

When we run kubectl apply -f pod.yaml:

Request goes to API Server

API Server validates the request

Data is stored in etcd

Scheduler assigns a node

kubelet ensures the pod runs

Container runtime starts the container

# Task 3 – kubectl Installation
 What is kubectl?

kubectl is a command-line tool used to interact with the Kubernetes cluster.

# Common kubectl Commands

kubectl get nodes – list all nodes

kubectl get pods – list pods

kubectl get pods -A – list pods in all namespaces

kubectl describe pod – detailed info  
