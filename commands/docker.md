<div align="center">

# 🐳 Docker Commands

</div>

[← Back to Home](../README.md)

---

## Table of Contents

- [Container Management](#-container-management)
- [Image Management](#-image-management)
- [Docker Compose](#-docker-compose)
- [Volumes & Networks](#-volumes--networks)
- [Registry & Hub](#-registry--hub)
- [Logs & Debugging](#-logs--debugging)
- [System Cleanup](#-system-cleanup)

---

## 🔲 Container Management

### Run a container

```bash
docker run <image>
```
> Pulls the image (if not local) and starts a new container.

**Common flags:**

| Flag | Description |
|------|-------------|
| `-d` | Run in detached (background) mode |
| `-p 8080:80` | Map host port `8080` → container port `80` |
| `--name my-app` | Give the container a name |
| `-e KEY=VALUE` | Set environment variables |
| `-v /host:/container` | Mount a volume |
| `--rm` | Automatically remove container when it stops |
| `-it` | Interactive terminal (bash/sh) |

**Examples:**

```bash
# Run nginx in the background on port 8080
docker run -d -p 8080:80 --name my-nginx nginx

# Run an interactive Ubuntu shell (removed after exit)
docker run -it --rm ubuntu bash

# Run with environment variables
docker run -d -e POSTGRES_PASSWORD=secret --name db postgres
```

---

### List containers

```bash
docker ps
```
> Lists all **running** containers.

```bash
docker ps -a
```
> Lists **all** containers (including stopped ones).

---

### Start / Stop / Restart a container

```bash
docker start <container>
docker stop <container>
docker restart <container>
```

**Example:**

```bash
docker stop my-nginx
docker start my-nginx
```

---

### Remove a container

```bash
docker rm <container>
```

```bash
# Force-remove a running container
docker rm -f <container>

# Remove all stopped containers
docker container prune
```

---

### Execute a command inside a running container

```bash
docker exec -it <container> <command>
```

**Examples:**

```bash
# Open a bash shell inside a container
docker exec -it my-nginx bash

# Run a one-off command
docker exec my-nginx ls /etc/nginx
```

---

### Copy files between host and container

```bash
# Host → Container
docker cp ./file.txt my-container:/app/file.txt

# Container → Host
docker cp my-container:/app/logs.txt ./logs.txt
```

---

### View container resource usage

```bash
docker stats
```

---

## 🖼️ Image Management

### List images

```bash
docker images
```

---

### Pull an image from Docker Hub

```bash
docker pull <image>:<tag>
```

```bash
docker pull nginx:latest
docker pull node:20-alpine
docker pull postgres:16
```

---

### Build an image from a Dockerfile

```bash
docker build -t <name>:<tag> <path>
```

```bash
# Build from current directory
docker build -t my-app:1.0 .

# Build with a specific Dockerfile
docker build -f Dockerfile.prod -t my-app:prod .
```

---

### Tag an image

```bash
docker tag <source> <target>
```

```bash
docker tag my-app:1.0 myregistry/my-app:1.0
```

---

### Remove an image

```bash
docker rmi <image>

# Remove all unused images
docker image prune -a
```

---

### Inspect an image

```bash
docker inspect <image>
```

---

## 🎼 Docker Compose

### Start services (detached)

```bash
docker compose up -d
```

---

### Stop services

```bash
docker compose down
```

```bash
# Also remove volumes
docker compose down -v
```

---

### View service logs

```bash
docker compose logs -f
docker compose logs -f <service>
```

---

### Rebuild and restart services

```bash
docker compose up -d --build
```

---

### List running compose services

```bash
docker compose ps
```

---

### Run a one-off command in a service

```bash
docker compose exec <service> <command>
```

```bash
docker compose exec web bash
docker compose exec db psql -U postgres
```

---

### Scale a service

```bash
docker compose up -d --scale web=3
```

---

## 💾 Volumes & Networks

### List volumes

```bash
docker volume ls
```

### Create a volume

```bash
docker volume create my-data
```

### Remove a volume

```bash
docker volume rm my-data

# Remove all unused volumes
docker volume prune
```

---

### List networks

```bash
docker network ls
```

### Create a network

```bash
docker network create my-network
```

### Connect a container to a network

```bash
docker network connect my-network my-container
```

### Inspect a network

```bash
docker network inspect my-network
```

---

## 🏪 Registry & Hub

### Login to Docker Hub

```bash
docker login
```

### Push an image

```bash
docker push <image>:<tag>
```

```bash
docker push myusername/my-app:1.0
```

### Login to a private registry

```bash
docker login registry.example.com
```

---

## 🔍 Logs & Debugging

### View container logs

```bash
docker logs <container>

# Follow live logs
docker logs -f <container>

# Last 100 lines
docker logs --tail 100 <container>
```

---

### Inspect container details

```bash
docker inspect <container>
```

---

### Get container IP address

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container>
```

---

### View container processes

```bash
docker top <container>
```

---

## 🧹 System Cleanup

### Remove everything unused (containers, images, volumes, networks)

```bash
docker system prune -a --volumes
```

> ⚠️ This will permanently delete all stopped containers, unused images, and unused volumes.

---

### Check disk usage

```bash
docker system df
```

---

[← Back to Home](../README.md) | [🐧 Linux →](linux.md)
