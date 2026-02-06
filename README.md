# Docker Installation and Usage Guide for Kali Linux

A comprehensive guide for installing and using Docker on Kali Linux Rolling (2025.2).

## 📋 Table of Contents

- [System Information](#system-information)
- [Installation](#installation)
- [Configuration](#configuration)
- [Verification](#verification)
- [Getting Started](#getting-started)
- [Docker Compose Example](#docker-compose-example)
- [Common Commands](#common-commands)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)

## 💻 System Information

- **OS**: Kali GNU/Linux Rolling 2025.2
- **Codename**: kali-rolling
- **Docker Version**: 27.5.1+dfsg4
- **Docker Compose Version**: 2.32.4-3

## 🚀 Installation

### Step 1: Update System Packages

```bash
sudo apt update
```

> **Note**: You may see warnings about Microsoft repository signatures. These can be safely ignored for Docker installation.

### Step 2: Install Required Dependencies

```bash
sudo apt install -y ca-certificates curl gnupg
```

### Step 3: Install Docker

On Kali Linux, Docker is available directly from the official Kali repositories:

```bash
sudo apt install -y docker.io docker-compose
```

This will install:
- `docker.io` - Docker Engine
- `docker-compose` - Docker Compose tool
- Additional dependencies (containerd, runc, etc.)

### Step 4: Verify Installation

```bash
# Check Docker version
docker --version

# Check Docker Compose version
docker compose version

# Check Docker daemon status
sudo systemctl status docker
```

**Expected Output:**
```
Docker version 27.5.1+dfsg4, build cab968b3
Docker Compose version 2.32.4-3
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: active (running)
```

## ⚙️ Configuration

### Add User to Docker Group

To run Docker commands without `sudo`:

```bash
# Add current user to docker group
sudo usermod -aG docker $USER

# Verify group membership
groups $USER

# Apply changes (choose one option):

# Option 1: Log out and log back in
# Option 2: Run in current terminal
newgrp docker
```

### Verify Permissions

```bash
# This should work without sudo
docker ps
```

## ✅ Verification

### Test Docker Installation

Run the hello-world container:

```bash
docker run hello-world
```

**Expected Output:**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### Check Running Containers

```bash
docker ps
```

## 🎯 Getting Started

### Basic Docker Commands

```bash
# List all containers (running and stopped)
docker ps -a

# List all images
docker images

# Pull an image from Docker Hub
docker pull nginx:alpine

# Run a container
docker run -d -p 8080:80 nginx:alpine

# Stop a container
docker stop <container_id>

# Remove a container
docker rm <container_id>

# Remove an image
docker rmi <image_name>
```

## 🐳 Docker Compose Example

### Create a Multi-Container Application

This example sets up an Nginx web server with a PostgreSQL database.

#### 1. Create Project Directory

```bash
mkdir docker-test
cd docker-test
```

#### 2. Create docker-compose.yml

```bash
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: postgres:alpine
    environment:
      POSTGRES_PASSWORD: example
EOF
```

#### 3. Create HTML Content

```bash
# Create directory
mkdir -p html

# Create index.html
echo '<h1>Docker Test Page</h1><p>Docker Compose is working!</p>' > html/index.html
```

#### 4. Start Services

```bash
# Start containers in detached mode
docker compose up -d

# Check running services
docker compose ps
```

#### 5. Test the Application

```bash
# Test web server
curl http://localhost:8080

# Or open in browser
xdg-open http://localhost:8080
```

**Expected Output:**
```
<h1>Docker Test Page</h1><p>Docker Compose is working!</p>
```

#### 6. Manage Services

```bash
# View logs
docker compose logs

# Stop services
docker compose down

# Restart services
docker compose restart

# Stop and remove volumes
docker compose down -v
```

## 📝 Common Commands

### Container Management

```bash
# Start a container
docker start <container_id>

# Stop a container
docker stop <container_id>

# Restart a container
docker restart <container_id>

# View container logs
docker logs <container_id>

# Execute command in running container
docker exec -it <container_id> /bin/sh

# Inspect container
docker inspect <container_id>
```

### Image Management

```bash
# List images
docker images

# Remove image
docker rmi <image_name>

# Remove unused images
docker image prune

# Pull image
docker pull <image_name>

# Build image from Dockerfile
docker build -t <tag_name> .
```

### System Cleanup

```bash
# Remove all stopped containers
docker container prune

# Remove all unused images
docker image prune -a

# Remove all unused volumes
docker volume prune

# Remove all unused networks
docker network prune

# Complete cleanup (use with caution!)
docker system prune -a --volumes
```

### Docker Compose Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f

# List services
docker compose ps

# Execute command in service
docker compose exec <service_name> <command>

# Rebuild services
docker compose up -d --build

# Scale services
docker compose up -d --scale <service_name>=3
```

## 🔧 Troubleshooting

### Docker Daemon Not Running

```bash
# Check status
sudo systemctl status docker

# Start Docker
sudo systemctl start docker

# Enable Docker on boot
sudo systemctl enable docker
```

### Permission Denied Error

If you get "permission denied" errors:

```bash
# Verify you're in docker group
groups $USER

# If not, add user to docker group
sudo usermod -aG docker $USER

# Log out and log back in, or run:
newgrp docker
```

### Port Already in Use

```bash
# Find process using port 8080
sudo lsof -i :8080

# Or use netstat
sudo netstat -tulpn | grep 8080

# Kill the process
sudo kill -9 <PID>
```

### Container Won't Start

```bash
# Check logs
docker logs <container_id>

# Inspect container
docker inspect <container_id>

# Check system resources
docker system df
```

## 📚 Resources

### Official Documentation

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Docker Hub](https://hub.docker.com/)

### Kali Linux Specific

- [Kali Linux Official Site](https://www.kali.org/)
- [Kali Linux Documentation](https://www.kali.org/docs/)

### Useful Links

- [Docker Cheat Sheet](https://docs.docker.com/get-started/docker_cheatsheet.pdf)
- [Best Practices for Writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Security Best Practices](https://docs.docker.com/engine/security/)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This guide is available under the MIT License.

## ✨ Acknowledgments

- Docker Team for creating an amazing containerization platform
- Kali Linux Team for maintaining excellent package repositories
- The open-source community

---

**Last Updated**: February 6, 2026  
**Author**: Subash  
**System**: Kali GNU/Linux Rolling 2025.2