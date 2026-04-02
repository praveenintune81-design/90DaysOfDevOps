#  Docker Cheat Sheet (Day 37)

##  Container Commands
docker run -it ubuntu bash          # Run interactive container
docker run -d nginx                 # Run in detached mode
docker ps                           # List running containers
docker ps -a                        # List all containers
docker stop <id>                    # Stop container
docker start <id>                   # Start container
docker rm <id>                      # Remove container
docker exec -it <id> bash           # Access running container
docker logs <id>                    # View logs

##  Image Commands
docker build -t myapp .             # Build image
docker images                       # List images
docker rmi <id>                     # Remove image
docker pull nginx                   # Pull image
docker push myrepo/myapp            # Push image
docker tag img repo:tag             # Tag image

##  Volume Commands
docker volume create myvol          # Create volume
docker volume ls                    # List volumes
docker volume inspect myvol         # Inspect volume
docker volume rm myvol              # Remove volume

##  Network Commands
docker network create mynet         # Create network
docker network ls                   # List networks
docker network inspect mynet        # Inspect network
docker network connect mynet cont   # Connect container

## Docker Compose
docker compose up -d                # Start services
docker compose down                 # Stop services
docker compose down -v              # Remove volumes
docker compose ps                   # List services
docker compose logs                 # View logs
docker compose build                # Build services

##  Cleanup Commands
docker system prune                 # Remove unused data
docker system prune -a              # Remove all unused images
docker volume prune                 # Remove unused volumes
docker system df                    # Check disk usage

##  Dockerfile Instructions
FROM node:18                        # Base image
WORKDIR /app                        # Working directory
COPY . .                            # Copy files
RUN npm install                     # Run command
EXPOSE 3000                         # Expose port
CMD ["node","app.js"]               # Default command
ENTRYPOINT ["node"]                 # Fixed command
