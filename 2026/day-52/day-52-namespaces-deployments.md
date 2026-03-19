Day 52 – Kubernetes Namespaces and Deployments
Overview

This document covers Kubernetes Namespaces and Deployments with hands-on commands, explanations, and real-world concepts.

1. What are Namespaces?
Namespaces are logical partitions inside a Kubernetes cluster used to organize and isolate resources.

Why use Namespaces..?

Environment separation (dev, staging, production)
Resource isolation
Access control (RBAC)
Default Namespaces

Namespace	Description
default	Default working namespace
kube-system	Kubernetes internal components
kube-public	Public resources
kube-node-lease	Node heartbeat tracking

Commands Used

kubectl get namespaces
kubectl get pods -n kube-system
Custom Namespaces

kubectl create namespace dev
kubectl create namespace staging

Verify:

kubectl get namespaces
Run Pods in Namespaces
kubectl run nginx-dev --image=nginx -n dev
kubectl run nginx-staging --image=nginx -n staging

Check:
kubectl get pods -A

2. What is a Deployment?

A Deployment ensures that a specified number of Pods are always running.

Deployment YAML
----------------------------------------------------------------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
-------------------------------------------------      
YAML Explanation
replicas: Number of pods

selector: Connects Deployment to Pods

template: Pod blueprint

containers: Container details

Apply Deployment

kubectl apply -f nginx-deployment.yml

Check:

kubectl get deployments -n dev
kubectl get pods -n dev

Self-Healing

Delete a pod:

kubectl delete pod <pod-name> -n dev

Kubernetes automatically recreates it.
 Scaling
 
Scale Up
kubectl scale deployment nginx-deployment --replicas=5 -n dev

Scale Down
kubectl scale deployment nginx-deployment --replicas=2 -n dev

Rolling Update

Update image:
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
Check rollout:
kubectl rollout status deployment/nginx-deployment -n dev

Rollback
kubectl rollout undo deployment/nginx-deployment -n dev
Verify:
kubectl describe deployment nginx-deployment -n dev | grep Image

Key Differences
Feature	Pod	Deployment
Self-healing	
Scaling	
Rolling Update	

 Cleanup

kubectl delete deployment nginx-deployment -n dev
kubectl delete namespace dev staging

Final Learning Summary
Namespaces organize cluster resources
Deployments manage Pods
Scaling adjusts load
Rolling updates ensure zero downtime
Rollback restores previous versions
