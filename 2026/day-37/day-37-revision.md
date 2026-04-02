#  Docker Revision Guide (Day 37)

#  1. What is Docker?

Docker is a containerization platform used to package applications with all dependencies into a container.

 Works same everywhere (local, server, cloud)


#  2. Image vs Container

| Image | Container |
|------|----------|
| Blueprint | Running instance |
| Read-only | Read + write |
| Static | Dynamic |

 Example:
Image = Node app  
Container = Running Node app


#  3. What happens to container data?

 Deleted when container removed  
 Persist using:
 Volumes
 Bind mounts


#  4. Image Layers & Caching

Each Dockerfile instruction = layer

Example:
FROM node
RUN npm install
COPY .

 Docker caches layers  
 Faster rebuilds

#  5. Dockerfile Deep Understanding

## Basic Structure

FROM → Base image  
WORKDIR → Working directory  
COPY → Copy files  
RUN → Execute commands  
CMD → Default command  

## CMD vs ENTRYPOINT

| CMD | ENTRYPOINT |
|-----|-----------|
| Default command | Fixed command |
| Can override | Cannot easily override |

 Best Practice:
Use both together

#  6. Build & Tag Image

docker build -t myapp:v1 .

 Tag format:
repo:version


#  7. Docker Hub

Push image:
docker push username/myapp

 Required:
- docker login
- public/private repo

#  8. Volumes

Used for persistent storage

docker volume create mydata

 Data survives container deletion

#  9. Bind Mounts

Local folder → container

docker run -v $(pwd):/app

Good for development

#  10. Networking

Custom network allows containers to talk via name

docker network create mynet

 Access using:
http://service-name

#  11. Docker Compose

Used for multi-container apps

Example:
- Node app
- MongoDB
- Nginx

## Important Keys

services:
build:
ports:
volumes:
environment:
depends_on:


#  12. Environment Variables

.env file:

PORT=3000
DB_HOST=mongo

Used in compose automatically

#  13. Multi-Stage Build

Used to reduce image size
Example:
Stage 1 → Build app  
Stage 2 → Run app

Only final stage shipped

#  14. Healthchecks

Used to check container status
healthcheck:
curl http://localhost

#  depends_on
Controls startup order
BUT:
Does NOT wait for service ready
#  15. Cleanup
docker system prune -a
 Removes:
- stopped containers
- unused images
- networks

#  16. Disk Usage

docker system df

#  17. Port Mapping

-p 8080:80

 Host:Container

Access:
localhost:8080 → container:80

#  18. COPY vs ADD

| COPY | ADD |
|------|-----|
| Simple copy | Extra features |
| Recommended | Rare use |

 Always prefer COPY

#  19. Communication Between Containers

Same network → use service name

Example:
mongodb://mongo:27017

#  20. docker compose down -v

| Command | Behavior |
|--------|---------|
| down | stops containers |
| down -v | removes volumes |


#  21. Why Multi-Stage Builds?

 Smaller image  
 Secure  
 Production ready  

#  Self-Assessment Answers

## Q1: Image vs Container?
Image = template  
Container = running instance  

## Q2: Data lost?
Yes, unless using volume  

## Q3: Communication?
Using container name in same network  

## Q4: down vs down -v?
-v deletes volumes  

## Q5: Multi-stage benefit?
Small + secure image  

## Q6: COPY vs ADD?
COPY is preferred  

## Q7: -p 8080:80?
Host port → container port  

## Q8: Disk usage?
docker system df  
