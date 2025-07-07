Here's a **complete Docker Cheatsheet** with commonly used commands for managing containers, images, volumes, networks, and more.

---

# 🐳 Docker Full Cheatsheet – One Command per Line

## 🔧 Installation & Basic Info

```bash
sudo apt update && sudo apt install docker.io          # Install Docker on Debian/Ubuntu/Parrot OS
docker --version                                      # Check Docker version
docker info                                           # Display system-wide information
docker --help                                         # Show help for Docker CLI
```

---

## 🐋 Managing Containers

```bash
docker run hello-world                                # Run a test container
docker run -it ubuntu bash                            # Run Ubuntu interactively
docker run -d --name mynginx -p 80:80 nginx           # Run Nginx in background (detached)
docker start <container_id_or_name>                   # Start a stopped container
docker stop <container_id_or_name>                    # Stop a running container
docker restart <container_id_or_name>                 # Restart a container
docker pause <container_id_or_name>                   # Pause a container
docker unpause <container_id_or_name>                 # Unpause a container
docker rm <container_id_or_name>                      # Remove a container
docker rm -f <container_id_or_name>                   # Force remove a running container
docker kill <container_id_or_name>                    # Kill a container instantly
docker logs <container_id_or_name>                    # View container logs
docker logs -f <container_id_or_name>                  # Follow logs (tail -f style)
docker exec -it <container_id> bash                   # Execute command inside running container
docker inspect <container_id_or_name>                 # Get detailed container info
docker top <container_id_or_name>                     # Show running processes in container
docker stats                                          # Show performance stats of all containers
```

---

## 📦 Managing Images

```bash
docker images                                         # List all local images
docker pull <image_name>                              # Pull image from registry (e.g., nginx)
docker build -t <image_name> .                        # Build image from Dockerfile in current dir
docker rmi <image_id_or_name>                         # Remove an image
docker rmi -f <image_id_or_name>                      # Force remove image
docker image inspect <image_id_or_name>               # Inspect image details
docker history <image_id_or_name>                     # Show history of an image
docker tag <image_id> <new_tag>                       # Tag an image
docker push <image_name>                              # Push image to remote registry
```

---

## 🗃️ Managing Volumes

```bash
docker volume ls                                      # List all volumes
docker volume create myvolume                         # Create a named volume
docker volume inspect myvolume                        # Inspect volume details
docker volume rm myvolume                             # Remove a volume
docker volume prune                                   # Remove unused local volumes
```

---

## 🌐 Managing Networks

```bash
docker network ls                                     # List all networks
docker network create mynetwork                       # Create a custom network
docker network inspect mynetwork                      # Inspect network details
docker network connect mynetwork <container>          # Connect container to network
docker network disconnect mynetwork <container>       # Disconnect container from network
docker network rm mynetwork                           # Remove a network
docker network prune                                  # Remove unused networks
```

---

## 🧹 Cleanup Commands

```bash
docker container prune                                # Remove all stopped containers
docker image prune                                    # Remove dangling images
docker image prune -a                                 # Remove all unused images
docker volume prune                                   # Remove unused volumes
docker network prune                                  # Remove unused networks
docker system prune                                   # Remove all unused data (containers, networks, images, volumes)
docker system prune -af                               # Aggressive full cleanup (no confirmation)
```

---

## 📁 Copying Files Between Host and Container

```bash
docker cp myfile.txt <container_id>:/path/to/dest     # Copy file from host to container
docker cp <container_id>:/path/to/file /host/path      # Copy file from container to host
```

---

## 🧪 Docker Compose (if installed)

```bash
docker-compose up                                     # Start services defined in docker-compose.yml
docker-compose up -d                                  # Start in detached mode
docker-compose down                                   # Stop and remove containers
docker-compose ps                                     # List running containers
docker-compose logs                                   # View logs
docker-compose build                                  # Build or rebuild services
docker-compose stop                                   # Stop running containers
docker-compose start                                  # Start already created containers
docker-compose restart                                # Restart services
docker-compose config                                 # Validate and view Compose file
docker-compose pull                                   # Pull service images
docker-compose rm                                     # Remove stopped containers
```

---

## 🧱 Dockerfile Quick Reference

| Instruction | Purpose |
|-----------|---------|
| `FROM` | Base image |
| `RUN` | Run command during build |
| `CMD` | Default command when container starts |
| `ENTRYPOINT` | Primary command (not overridden by CMD) |
| `COPY` | Copy files from host into image |
| `ADD` | Like COPY, but supports URLs and auto-extraction |
| `WORKDIR` | Set working directory |
| `EXPOSE` | Expose port (metadata only) |
| `ENV` | Set environment variables |
| `VOLUME` | Define mount point for persistent data |
| `LABEL` | Add metadata |

---

## 🧷 Advanced Tips

```bash
docker inspect --format='{{.NetworkSettings.IPAddress}}' <container_id>   # Get container IP
docker run --rm alpine ping 8.8.8.8                     # Run temporary container and remove after use
docker run --network host ubuntu                      # Use host networking
docker run --privileged                               # Give extended privileges
docker run --cap-add=NET_ADMIN                        # Add specific Linux capabilities
docker save <image> > backup.tar                      # Export image as tar archive
docker load < backup.tar                              # Import saved image
```

---

## ✅ Summary

| Task | Command |
|------|---------|
| Run container | `docker run <image>` |
| List containers | `docker ps` |
| List all containers | `docker ps -a` |
| Build image | `docker build -t <name> .` |
| Remove container | `docker rm <id>` |
| Remove image | `docker rmi <id>` |
| Clean unused stuff | `docker system prune` |
| Manage multiple services | `docker-compose up` |

---

Let me know if you want:
- A printable PDF version
- Dockerfile examples
- Help with multi-stage builds or security hardening

Happy Dockering! 🐳💻
