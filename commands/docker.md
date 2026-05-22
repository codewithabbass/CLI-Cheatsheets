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
- [Build & Multi-platform (Buildx)](#-build--multi-platform-buildx)
- [Security Scanning (Scout)](#-security-scanning-scout)
- [Node.js Dockerization](#-nodejs-dockerization)
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
# Interactive (allocates a TTY — use for shells)
docker exec -it <container> <command>

# Non-interactive (for one-off commands)
docker exec <container> <command>
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

### List images (equivalent commands)

```bash
docker images
```

```bash
docker image ls
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

## 🔧 Build & Multi-platform (Buildx)

> Docker Buildx uses BuildKit for advanced build features, including multi-platform images.

```bash
# Create a new builder with multi-platform support
docker buildx create --name mybuilder --use

# Check available builders
docker buildx ls

# Build and push a multi-platform image (linux/amd64 + linux/arm64)
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t myuser/my-app:latest \
  --push .

# Build for local use (single platform)
docker buildx build --load -t my-app:latest .

# Inspect a multi-platform manifest
docker buildx imagetools inspect myuser/my-app:latest

# Remove builder
docker buildx rm mybuilder
```

---

## 🔍 Security Scanning (Scout)

> `docker scout` provides vulnerability scanning for images (requires Docker Desktop or `docker scout` plugin).

```bash
# Quick vulnerability overview
docker scout quickview my-app:latest

# Full CVE list
docker scout cves my-app:latest

# Recommendations for fixing issues
docker scout recommendations my-app:latest

# Compare two images
docker scout compare my-app:v1 my-app:v2

# Scan image from Docker Hub
docker scout quickview nginx:latest
```

---

## 🟢 Node.js Dockerization

### Dockerfile (production-ready)

```dockerfile
# Use official Node.js LTS image on Alpine (small footprint)
FROM node:20-alpine AS base

# Set working directory
WORKDIR /app

# Copy package files first (leverages layer caching)
COPY package*.json ./

# Install production dependencies only
RUN npm ci --omit=dev

# Copy the rest of the source code
COPY . .

# Expose the port your app listens on
EXPOSE 3000

# Run as non-root user for security
USER node

# Start the application
CMD ["node", "server.js"]
```

> ✅ **Best practices applied:** Alpine base image, `npm ci` for reproducible installs, layer caching via early COPY of `package*.json`, non-root `USER node`.

---

### Multi-stage Dockerfile (with build step)

> Use this when you have a TypeScript build, bundler (Vite, esbuild), or similar compile step.

```dockerfile
# ── Stage 1: Build ──────────────────────────────────────────
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build          # e.g. tsc, vite build, etc.

# ── Stage 2: Production image ───────────────────────────────
FROM node:20-alpine AS production

WORKDIR /app

COPY package*.json ./
RUN npm ci --omit=dev

# Copy only the compiled output from the builder stage
COPY --from=builder /app/dist ./dist

EXPOSE 3000
USER node

CMD ["node", "dist/server.js"]
```

---

### .dockerignore

> Always include a `.dockerignore` to keep images lean and avoid leaking secrets.

```
node_modules
npm-debug.log
dist
.env
.env.*
.git
.gitignore
*.md
coverage
.nyc_output
```

---

### docker-compose.yml (Node.js + PostgreSQL)

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://postgres:secret@db:5432/mydb
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  db_data:
```

---

### Common Node.js Docker workflow

```bash
# 1. Build the image
docker build -t my-node-app:latest .

# 2. Run the container (map port, pass env file)
docker run -d \
  -p 3000:3000 \
  --env-file .env \
  --name my-node-app \
  my-node-app:latest

# 3. Stream logs
docker logs -f my-node-app

# 4. Open a shell for debugging
docker exec -it my-node-app sh

# 5. Run npm scripts inside the container
docker exec my-node-app npm run migrate

# 6. Rebuild after code changes (Compose)
docker compose up -d --build

# 7. Remove container and image when done
docker rm -f my-node-app
docker rmi my-node-app:latest
```

---

### Dev mode with live reload (bind mount)

```bash
docker run -it --rm \
  -p 3000:3000 \
  -v "$(pwd)":/app \
  -v /app/node_modules \
  -e NODE_ENV=development \
  --name my-node-dev \
  node:20-alpine \
  sh -c "npm install && npm run dev"
```

> The `-v /app/node_modules` anonymous volume prevents the host's `node_modules` from overwriting the container's.

---

[← Back to Home](../README.md) | [🐧 Linux →](linux.md)
