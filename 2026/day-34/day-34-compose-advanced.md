# Day 34 – Docker Compose: Advanced Multi-Container Application

##  Objective

The goal of this project is to build a production-like multi-container application using Docker Compose.
This includes a web application, database, and cache with proper dependency management, restart policies, networking, and scaling behavior.


#  Project Architecture

This project consists of three services:

* **Web Application (Node.js)** → Handles HTTP requests
* **PostgreSQL Database** → Stores persistent data
* **Redis Cache** → Improves performance using in-memory caching

### Flow
User → Web App → Redis (cache)
→ PostgreSQL (database)

#  Project Structure
day-34/
 ├── app/
 │    ├── app.js
 │    ├── package.json
 │    └── Dockerfile
 ├── docker-compose.yml
 └── day-34-compose-advanced.md

# Task 1: Build a Multi-Service Application

## What We Did

We created a 3-service architecture using Docker Compose:

* Web (Node.js)
* Database (PostgreSQL)
* Cache (Redis)

## Key Concept

Each service runs in its own container.
This follows microservices principles and improves scalability and maintainability.

## Important Insight

Docker Compose provides built-in DNS:

* Service name = hostname

Example:
host: "db"

#  Task 2: depends_on and Healthchecks

## Problem

Containers may start before dependent services are ready.

Example:

* App starts
* Database is still initializing
* Connection fails (ECONNREFUSED)

## Solution

### Healthcheck

We added a healthcheck to PostgreSQL:

healthcheck:
  test: ["CMD-SHELL", "pg_isready -U postgres"]
  interval: 5s
  timeout: 5s
  retries: 5

This ensures Docker checks whether the database is ready.

### depends_on with condition

depends_on:
  db:
    condition: service_healthy

Now the web app starts **only after the database is healthy**.

## Key Learning

Container started ≠ Service ready
Healthchecks are required for real dependency control.

# Task 3: Restart Policies

## Objective

Ensure services recover automatically from failures.

### Configuration
restart: always
### Behavior

| Scenario      | always  | on-failure |
| ------------- | ------- | ---------- |
| Crash         | Restart | Restart    |
| Manual stop   | Restart | No restart |
| System reboot | Restart | Restart    |

## Real Usage

* Database → always (critical service)
* Web App → on-failure
* Cache → always

## Key Learning

Restart policies make systems **self-healing**.

# Task 4: Custom Dockerfile and Build Process

## Objective

Build the web application using a Dockerfile instead of a prebuilt image.

### Dockerfile Flow

Dockerfile → Image → Container

## Rebuild Command

docker compose up --build

This ensures:

* Code changes are included
* New image is built
* Containers are recreated
  
## Build Cache Insight

Docker caches layers:

* If `package.json` is unchanged → dependencies are not reinstalled
* This speeds up builds


## Key Learning

Docker Compose acts like a **mini CI/CD pipeline**.


#  Task 5: Networks, Volumes, and Labels

## Networks

We defined a custom network:

networks:
  backend:

### Why?

* Isolates services
* Secure communication
* Controlled environment

## Volumes

volumes:
  db_data:


Attached to PostgreSQL:

volumes:
  - db_data:/var/lib/postgresql/data

### Why?

* Prevents data loss
* Persists data even after container removal

## Labels

labels:
  - "app=web"

### Use Cases

* Monitoring
* Logging
* Filtering containers

## Key Learning

* Networks = communication control
* Volumes = data persistence
* Labels = service organization

# Task 6: Scaling and Limitations

## Command

docker compose up --scale web=3

## Problem
Port conflict occurs:
ports:
  - "3000:3000"
Only one container can bind to port 3000.

## Result
* One container runs successfully
* Others fail due to port conflict

## Temporary Fix
Remove port mapping:
# remove ports
Now multiple containers can run.

## New Problem

Application is not accessible externally.

## Real Solution

Use a load balancer like Nginx

## Key Learning

Docker Compose can scale containers but **cannot distribute traffic**.

#  Final Summary

This project demonstrated:

* Multi-container architecture
* Service dependency management
* Healthchecks and readiness
* Restart policies for reliability
* Custom image builds
* Persistent storage using volumes
* Network isolation
* Scaling limitations

#  Conclusion

Docker Compose is powerful for local development and simulating production environments.
However, for real production scaling, tools like load balancers and orchestration systems are required.

#  Commands Used
docker compose up --build
docker compose down
docker compose restart
docker compose up --scale web=3
docker ps
docker logs

