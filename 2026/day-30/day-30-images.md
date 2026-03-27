# Day 30 – Docker Images & Container Lifecycle

##  Objective

Understand Docker images, layers, and container lifecycle with hands-on practice.

## Task 1: Docker Images

### Pull Images

docker pull nginx
docker pull ubuntu
docker pull alpine

### List Images

docker images

### Observation

* Ubuntu image is large (~70MB+)
* Alpine image is very small (~5MB)

### Reason

Alpine is a minimal Linux distribution using BusyBox, while Ubuntu includes full OS utilities.

### Inspect Image

docker inspect nginx

### Remove Image

docker rmi ubuntu

## 🔹 Task 2: Image Layers

### Command

docker image history nginx

### Observation

* Each line represents a layer
* Some layers show `0B` (metadata layers)

### What are Layers?

Docker images are built in layers. Each instruction in a Dockerfile creates a layer.

### Why Layers?

* Reusability
* Faster builds (cache)
* Efficient storage

##  Task 3: Container Lifecycle

### Steps

docker create --name my-container nginx
docker start my-container
docker pause my-container
docker unpause my-container
docker stop my-container
docker restart my-container
docker kill my-container
docker rm my-container

### Lifecycle States

* Created
* Running
* Paused
* Stopped
* Removed


##  Task 4: Running Containers

### Run Container

docker run -d -p 8080:80 --name web nginx

### Logs

docker logs web
docker logs -f web

### Exec into Container

docker exec -it web /bin/bash

### Run Command

docker exec web ls /

### Inspect Container

docker inspect web

##  Task 5: Cleanup

docker stop $(docker ps -q)
docker rm $(docker ps -aq)
docker image prune -a
docker system df

##  Key Learnings

* Docker Image = Blueprint
* Container = Running Instance
* Layers improve performance and storage
* Alpine is lightweight compared to Ubuntu
* Container lifecycle defines state transitions
* Logs and exec help debug containers

## Conclusion

Today we learned how Docker images are structured, how containers move through different states, and how to interact with running containers effectively. This knowledge is fundamental for working with Docker in real-world DevOps environments.
