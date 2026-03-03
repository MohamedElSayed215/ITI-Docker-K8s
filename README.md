# 🐳 ITI — Docker Labs

> Hands-on lab work from the **Docker Track** at the **Information Technology Institute (ITI)**.  
> The course covered Docker internals, image building, networking, orchestration, and more across **3 practical labs**.

---

## 📁 Repository Structure

```
ITI-Docker-K8s/
├── Lab1/          # Docker Internals & Getting Started
├── Lab2/          # Images, Dockerfile, Volumes & Private Registry
├── Lab3/          # Networking & Docker Compose
└── README.md
```

---

## 🧪 Lab 1 — Docker Internals & Getting Started

Understanding the Linux kernel building blocks that power Docker, followed by installation and first container operations.

**Topics Covered:**

- **Namespaces** — OS-level isolation for processes (PID, NET, MNT, UTS, IPC, USER)
- **Cgroups** — resource control and limits (CPU, memory, I/O)
- **Union Filesystems** — layered storage driver (OverlayFS)
- Docker Engine installation & daemon configuration
- Running, inspecting, and removing containers

---

## 🧪 Lab 2 — Images, Dockerfile, Volumes & Private Registry

Building custom images, persisting data, and hosting a private image registry.

**Topics Covered:**

- Pulling & inspecting images from Docker Hub
- Writing optimized **Dockerfiles** (layer caching, `.dockerignore`, multi-stage builds)
- Container lifecycle management (`run`, `exec`, `stop`, `rm`)
- **Docker Volumes** — named volumes vs bind mounts
- Data persistence and sharing volumes between containers
- Setting up and using a **Private Registry** (`registry:2`)

---

## 🧪 Lab 3 — Networking & Docker Compose

Connecting containers and orchestrating multi-service applications.

**Topics Covered:**

- Docker **network drivers** — bridge, host, none
- Container-to-container communication & DNS resolution
- Exposing ports and managing traffic between containers
- Writing `docker-compose.yml` for multi-service apps
- **Docker Compose** commands: `up`, `down`, `logs`, `ps`, `scale`
- Defining networks and volumes inside Compose files

---

## 🛠️ Prerequisites

- Docker Engine → [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose → [Install Compose](https://docs.docker.com/compose/install/)
- Linux recommended (Ubuntu 22.04 LTS used during the course)

---

## 🚀 Getting Started

```bash
# Clone the repo
git clone https://github.com/MohamedElSayed215/ITI-Docker-K8s.git
cd ITI-Docker-K8s

# Navigate to a lab
cd Lab1
```

---

## 📋 Quick Commands Reference

### 🖼️ Images
```bash
docker pull <image>                  # Pull from Docker Hub
docker build -t <name>:<tag> .       # Build from Dockerfile
docker images                        # List local images
docker rmi <image>                   # Remove an image
docker history <image>               # View image layers
```

### 📦 Containers
```bash
docker run -d -p 8080:80 <image>     # Run detached with port mapping
docker ps -a                         # List all containers
docker exec -it <container> bash     # Shell into container
docker logs -f <container>           # Follow container logs
docker inspect <container>           # Detailed container info
docker stop <container>              # Stop container
docker rm <container>                # Remove container
```

### 💾 Volumes
```bash
docker volume create <name>          # Create named volume
docker volume ls                     # List volumes
docker volume inspect <name>         # Inspect volume details
docker run -v <name>:/path <image>   # Mount named volume
docker run -v /host:/container <img> # Bind mount
```

### 🌐 Networking
```bash
docker network create <name>         # Create a network
docker network ls                    # List networks
docker network inspect <name>        # Inspect network
docker run --network <name> <image>  # Run on specific network
docker network connect <net> <ctr>   # Connect container to network
```

### 📦 Private Registry
```bash
# Run local registry
docker run -d -p 5000:5000 --name registry registry:2

# Tag and push image
docker tag <image> localhost:5000/<image>
docker push localhost:5000/<image>

# Pull from local registry
docker pull localhost:5000/<image>
```

### 🎼 Docker Compose
```bash
docker compose up -d                 # Start all services
docker compose down                  # Stop and clean up
docker compose logs -f               # Follow logs
docker compose ps                    # List running services
docker compose build                 # Rebuild images
docker compose exec <svc> bash       # Shell into a service
```

---

## 🔬 Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Namespaces** | Isolate processes — each container gets its own PID, NET, MNT, etc. |
| **Cgroups** | Limit & monitor resource usage per container |
| **Union FS** | Efficient layered image storage (OverlayFS) |
| **Dockerfile** | Blueprint for building custom images |
| **Volumes** | Persist data outside the container lifecycle |
| **Bridge Network** | Default isolated network for container communication |
| **Compose** | Define and run multi-container apps declaratively |
| **Private Registry** | Self-hosted image storage using `registry:2` |

---

## 👤 Author

**Mohamed El Sayed**  
ITI Trainee — System Administration Track  
[GitHub](https://github.com/MohamedElSayed215)

---

> *"Build once, run anywhere."* — Docker Motto
