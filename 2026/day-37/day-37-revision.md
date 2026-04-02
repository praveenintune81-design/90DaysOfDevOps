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

# Docker Compose – Deep Revision


#  What is Docker Compose?

Docker Compose is a tool used to define and run **multi-container applications** using a YAML file.

Instead of running multiple docker run commands  
You define everything in one file → docker-compose.yml

#  Why Compose?

Without Compose:
docker run node  
docker run mongo  
docker run nginx  

 Hard to manage  
 No networking control  
 No dependency handling  

With Compose:
One command → full app runs  
Auto networking  
Clean structure  


#  docker-compose.yml Structure

version: "3"

services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    environment:
      - DB_HOST=mongo
    depends_on:
      - mongo

  mongo:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:

#  Key Concepts

## 1. services
Each container = one service

## 2. build vs image
build → build from Dockerfile  
image → pull from Docker Hub  

## 3. ports
Host:Container mapping

## 4. volumes
Persist data

## 5. environment
Pass env variables

## 6. depends_on
Controls start order

 IMPORTANT:
depends_on does NOT wait for DB ready

#  Networking in Compose

 All services automatically connected  
 Use service name as hostname

Example:
mongodb://mongo:27017

#  Real Example (Node + Mongo)

app connects to mongo using:
DB_HOST=mongo

 No IP needed


# Common Commands Explained

## Start App
docker compose up -d

## Stop App
docker compose down

## Remove Everything (including data)
docker compose down -v

## Logs
docker compose logs -f

## Enter Container
docker compose exec app bash

#  Common Interview Questions

## Q1: Difference between docker run and compose?
docker run → single container  
compose → multi-container  

## Q2: Why use Compose?
Automation + scalability + readability  

## Q3: How containers communicate?
Using service name  
## Q4: depends_on limitation?
Does not wait for readiness  

## Q5: Where env variables stored?
.env file 

#  Pro Tips
Always use .env file  
Use named volumes for DB  
Use depends_on + healthcheck  
Never hardcode credentials  

#  Summary
Docker Compose =  
 Define  
 Run  
 Manage  
 Multi-container apps easily  

