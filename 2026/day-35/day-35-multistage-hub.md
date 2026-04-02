#  Day 35 – Multi-Stage Builds & Docker Hub (Deep Explanation)


#  Objective

The goal of this task is to:

* Understand why Docker images become large
* Learn how to optimize images using multi-stage builds
* Push images to Docker Hub
* Apply real-world best practices used in production

#  Task 1: Problem with Large Docker Images

##  What happens in a normal Docker build?

When we use a simple Dockerfile like:

FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "app.js"]


This creates a **single-stage image**

###  Problems:

1. **Large Image Size**

   * Full OS + Node + build tools included
   * Can go up to **800MB–1GB**

2. **Unnecessary Files**

   * Source code + dependencies + cache all inside image

3. **Security Risks**

   * More packages = more vulnerabilities

4. **Slow Deployment**

   * Larger image → slow pull/push

##  Practical Example

### Step 1: Create App

mkdir day-35 && cd day-35
 app.js
console.log("Hello Day 35 ");


### Step 2: Single Stage Dockerfile

FROM node:18

WORKDIR /app

COPY . .

RUN npm init -y

CMD ["node", "app.js"]

### Step 3: Build & Check Size

docker build -t node-single .
docker images

 You will see a **very large image (~900MB)**

##  Conclusion (Important)

Single-stage builds:

* Mix build + runtime
* Keep unnecessary tools
* Not production-friendly

#  Task 2: Multi-Stage Build (Core Concept)

##  What is Multi-Stage Build?

Multi-stage builds allow us to:

* Use one container to **build**
* Use another container to **run**

## Real Concept Flow

[ Stage 1: Builder ]
- Install dependencies
- Build app

        ↓ COPY

[ Stage 2: Runtime ]
- Minimal image
- Only required files

##  Multi-Stage Dockerfile
# Stage 1: Builder
FROM node:18 AS builder

WORKDIR /app
COPY . .
RUN npm init -y

# Stage 2: Runtime
FROM node:18-alpine

WORKDIR /app

COPY --from=builder /app /app

CMD ["node", "app.js"]

##  Build & Compare

docker build -t node-multi .
docker images

##  Size Comparison

| Build Type   | Approx Size |
| ------------ | ----------- |
| Single Stage | ~900MB      |
| Multi-Stage  | ~150MB      |


## Why Multi-Stage is Smaller?

1. Alpine image is lightweight
2. Build tools removed
3. Only required files copied
4. Clean runtime environment


## Key Learning

Multi-stage = **Production standard approach**

#  Task 3: Push Image to Docker Hub

##  What is Docker Hub?

Docker Hub is a **public registry** where you can:

* Store images
* Share with team
* Deploy anywhere


##  Steps

### 1. Login

docker login

### 2. Tag Image
docker tag node-multi yourusername/node-app:v1
username/repository:tag
### 3. Push Image
docker push yourusername/node-app:v1

### 4. Verify
docker rmi yourusername/node-app:v1
docker pull yourusername/node-app:v1

This ensures image is correctly uploaded

##  Key Learning

* Tagging is required before pushing
* Docker Hub acts like GitHub for images

# Task 4: Docker Hub Repository

##  What to Explore

After pushing:

1. Open Docker Hub
2. Go to your repository
3. Add description


## Tags Understanding

| Tag    | Meaning         |
| ------ | --------------- |
| latest | default version |
| v1     | version 1       |
| v2     | updated version |

##  Example

docker pull yourusername/node-app:latest
docker pull yourusername/node-app:v1

##  Important

 Avoid using `latest` in production
 Always use version tags (v1, v2)


#  Task 5: Image Best Practices

## Improved Dockerfile
FROM node:18-alpine

WORKDIR /app

# Create non-root user
RUN addgroup appgroup && adduser -S appuser -G appgroup

COPY . .

RUN npm init -y

USER appuser

CMD ["node", "app.js"]

##  Best Practices Explained

### 1. Use Minimal Base Image
* alpine is smaller than ubuntu
* reduces attack surface

### 2. Avoid Root User
* root = security risk
* use USER for safety
### 3. Reduce Layers
RUN apt update
RUN apt install
RUN apt update && apt install
### 4. Use Specific Tags
node:latest
node:18-alpine

### 5. Keep Images Small
* Faster deploy
* Lower storage cost
* Better performance

# Final Project Structure

2026/day-35/

├── app.js
├── Dockerfile.single
├── Dockerfile.multi
├── Dockerfile.best
└── day-35-multistage-hub.md
#  Final Conclusion
Multi-stage builds are critical for:
* Reducing image size
* Improving security
* Optimizing performance
Docker Hub enables:
* Easy sharing
* Version control
* Deployment flexibility
