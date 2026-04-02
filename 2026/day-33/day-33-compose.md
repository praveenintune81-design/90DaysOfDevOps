#  Day 33 – Docker Compose: Multi-Container Basics

##  Objective

The goal of this task is to understand how to run multi-container applications using Docker Compose. Instead of running containers manually, 
Docker Compose allows us to define and manage everything in a single YAML file.

##  What is Docker Compose?

Docker Compose is a tool used to define and run multi-container Docker applications using a `docker-compose.yml` file.
With a single command:

docker compose up
All services, networks, and volumes are automatically created and started.


## ⚙️ Task 1 – Install & Verify

Docker Compose is installtion process 

# Step 1: Install required packages
sudo apt update
sudo apt install -y ca-certificates curl gnupg
# Step 2: Add Docker official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# Step 3: Add Docker repo
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Step 4: Install Docker Compose plugin
sudo apt update
sudo apt install -y docker-compose-plugin

# Verify installation
docker compose version
Docker Compose version v5.1.1

##  Task 2 – Single Container (Nginx)

### docker-compose.yml

services:
  nginx:
    image: nginx:latest
    container_name: my-nginx
    ports:
      - "8080:80"

### Commands

docker compose up -d
docker compose down

### Access

http://localhost:8080

## Task 3 – Multi-Container (WordPress + MySQL)

### docker-compose.yml
services:
  db:
    image: mysql:5.7
    container_name: wordpress-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress-app
    restart: always
    ports:
      - "8081:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
### Run

docker compose up -d

### Access

http://localhost:8081

### Persistence Test

docker compose down
docker compose up -d
Data should persist after restart.

##  Task 4 – Important Commands

| Command                       | Description             |
| ----------------------------- | ----------------------- |
| docker compose up -d          | Start services          |
| docker compose down           | Stop and remove         |
| docker compose stop           | Stop only               |
| docker compose ps             | List running containers |
| docker compose logs -f        | View logs               |
| docker compose logs <service> | Service logs            |
| docker compose up --build     | Rebuild images          |

## 🔐 Task 5 – Environment Variables

### .env file

MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=wordpress
MYSQL_USER=wpuser
MYSQL_PASSWORD=wppass

### docker-compose.yml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
  MYSQL_DATABASE: ${MYSQL_DATABASE}
  MYSQL_USER: ${MYSQL_USER}
  MYSQL_PASSWORD: ${MYSQL_PASSWORD}

## Project Structure

2026/
 └── day-33/
      ├── docker-compose-nginx.yml
      ├── docker-compose-wordpress.yml
      ├── docker-compose-env.yml
      └── day-33-compose.md

##  Key Learnings

* Docker Compose simplifies multi-container setup
* Service names act as DNS between containers
* Volumes ensure data persistence
* Environment variables improve flexibility and security
* Compose automatically creates networks

## 🏁 Conclusion

Docker Compose is an essential DevOps tool that helps manage multi-container applications efficiently. It reduces manual work and ensures consistency across environments.

