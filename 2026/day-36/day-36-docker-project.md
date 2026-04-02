# Day 36 – Docker Project: Full Application Dockerization

##  Project Overview

This project demonstrates how to Dockerize a full-stack application using:

* Node.js (Express)
* MongoDB
* Docker & Docker Compose

The goal is to containerize the application and make it portable and production-ready.


##  Architecture

User → Node.js App → MongoDB

##  Why This App?

I chose a Node.js + MongoDB application because:

* It reflects real-world backend systems
* Easy to scale and containerize
* Common in DevOps interviews

##  Dockerfile Explanation

# Stage 1: Build dependencies
FROM node:20-alpine AS builder
WORKDIR /app

# Copy package files
COPY app/package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY app/ .

# Stage 2: Production image
FROM node:20-alpine

WORKDIR /app

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy from builder stage
COPY --from=builder /app /app

# Switch user
USER appuser

# Expose port
EXPOSE 3000

# Start app
CMD ["node", "server.js"]

##  Docker Compose

version: "3.9"

services:
  app:
    build: .
    ports:
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      - mongo

  mongo:
    image: mongo:6
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:

##  Environment Variables

PORT=3000
MONGO_URI=mongodb://mongo:27017/mydb

##  How to Run

docker compose up --build

##  Testing

* Open browser: http://localhost:3000
* Verify MongoDB connection logs


##  Docker Hub

Image: https://hub.docker.com/r/yourdockerhubusername/node-app

##  Challenges Faced

### 1. MongoDB Connection Issue

**Problem:** App couldn't connect to DB
**Solution:** Used service name `mongo` instead of localhost

### 2. Image Size
**Problem:** Large image size
**Solution:** Used Alpine + multi-stage build

### 3. Permission Issues

**Problem:** Security risks
**Solution:** Used non-root user

##  Final Image Size
~150MB (optimized)

## Conclusion

This project demonstrates how to:

* Dockerize a backend application
* Use multi-container architecture
* Manage services using Docker Compose
* Push and deploy via Docker Hub
