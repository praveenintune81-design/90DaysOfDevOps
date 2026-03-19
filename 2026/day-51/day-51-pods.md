#  Day 51 – Kubernetes Pods & Manifests

#  1. What is a Kubernetes Manifest?

A Kubernetes manifest is a YAML file that defines the desired state of a resource.

Kubernetes reads this file and ensures the system matches the defined configuration.

#  2. Anatomy of a Kubernetes Manifest

Every manifest contains four main fields:

## 1. apiVersion

Defines the API version used.

Example:

apiVersion: v1


## 2. kind

Defines the resource type.

Example:

kind: Pod


## 3. metadata

Contains identifying data like name and labels.

Example:

metadata:
  name: nginx-pod
  labels:
    app: nginx


## 4. spec

Defines the desired state (what should run).

Example:

spec:
  containers:
  - name: nginx
    image: nginx


# 3. Pod Manifests

##  Nginx Pod

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80

##  BusyBox Pod

apiVersion: v1
kind: Pod
metadata:
  name: busybox-pod
  labels:
    app: busybox
    environment: dev
spec:
  containers:
  - name: busybox
    image: busybox:latest
    command: ["sh", "-c", "echo Hello from BusyBox && sleep 3600"]

## Multi Label Pod

apiVersion: v1
kind: Pod
metadata:
  name: multi-label-pod
  labels:
    app: myapp
    environment: staging
    team: devops
spec:
  containers:
  - name: nginx
    image: nginx

#  4. Imperative vs Declarative

## Imperative

Direct command-based approach.

Example:

kubectl run redis-pod --image=redis


* Quick
* Not reusable
* Not recommended for production


## Declarative

YAML-based approach.

Example:

kubectl apply -f pod.yaml

* Reusable
* Version controlled
* Best for production


#  5. Key Commands Used

kubectl get pods
kubectl describe pod <name>
kubectl logs <name>
kubectl exec -it <name> -- /bin/sh
kubectl apply -f file.yaml
kubectl delete pod <name>

#  6. Labels and Filtering

Labels are key-value pairs used to organize resources.

Examples:

kubectl get pods --show-labels
kubectl get pods -l app=nginx
kubectl get pods -l environment=dev

# 7. What happens when you delete a Pod?

When a standalone Pod is deleted:

* It is permanently removed
* It is NOT recreated automatically
* There is no controller managing it

This is why Deployments are used in production.

#  8. Learning Summary

* Learned Kubernetes Pod structure
* Understood YAML manifests
* Practiced pod creation
* Used labels and filtering
* Understood imperative vs declarative approach
* Learned container lifecycle behavior

# Conclusion

Kubernetes works on the concept of desired state.

You define what you want using YAML, and Kubernetes ensures that state is maintained.

# YAML FILES
1:-  nginx-pod.yml
----------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
  ----------------------------------
  2:- busybox-pod.yml
  ------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: busybox-pod
  labels:
    app: busybox
    environment: dev
spec:
  containers:
  - name: busybox
    image: busybox:latest
    command: ["sh", "-c", "echo Hello from BusyBox && sleep 3600"]
  ------------------------------------------------------------------
3:- multi-label-pod.yml
--------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: multi-label-pod
  labels:
    app: myapp
    environment: staging
    team: devops
spec:
  containers:
  - name: nginx
    image: nginx
--------------------------------------------------------------
