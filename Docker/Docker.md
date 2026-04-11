---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - Containers
  - Virtualization
source: GeeksforGeeks, Docker Docs
---

> [!quote] Give it all you've got because you never know if there's going to be a next time.
> — Danielle Ingrum

# Docker: Complete Reference Guide

> [!info] Table of Contents
> - [[#Introduction & Basics]]
> - [[#Images & Dockerfiles]]
> - [[#Containers & CLI]]
> - [[#Docker Hub & Registry]]
> - [[#Docker Compose]]
> - [[#Volumes & Storage]]
> - [[#Networking]]

---

Docker is a tool that allows you to **package an application and everything it needs** (code, libraries, dependencies, configuration) into a single unit called a **`container`**. This container can run **the same way on any machine** that has Docker installed.

> [!tip] Why Docker?
> - Solves "it works on my machine" problem
> - Eliminates dependency conflicts
> - Lightweight (shares OS kernel, not full OS)
> - Fast startup (seconds vs minutes)

---

## Quick Comparison: Containers vs VMs

| Aspect | Docker Container | Virtual Machine |
|--------|--------------|---------------|
| **Weight** | Lightweight (MBs) | Heavy (GBs) |
| **Startup** | Seconds | Minutes |
| **Isolation** | Process level | Hardware level |
| **OS** | Shares host kernel | Full guest OS |

---

## Introduction & Basics
[[Docker 1 - Introduction & Architecture]]

> [!summary] Topics Covered:
> - What is Docker and why it matters
> - Problems Docker solves (standardization, isolation)
> - Docker Engine architecture (Client, Daemon, API)
> - Container isolation mechanisms (namespaces, cgroups)
> - Containers vs Virtual Machines comparison

[[Docker 1 - Introduction & Architecture]]

---

## Images & Dockerfiles 
[[Docker 2 - Images & Dockerfiles]]

> [!summary] Topics Covered:
> - Docker images (read-only templates)
> - Image layers and caching
> - Dockerfile syntax and instructions
> - CMD vs ENTRYPOINT vs RUN
> - Multi-stage builds
> - .dockerignore and security best practices

[[Docker 2 - Images & Dockerfiles]]

---

## Containers & CLI
[[Docker 3 - Containers & CLI]]

> [!summary] Topics Covered:
> - Container lifecycle states
> - Docker CLI commands (run, start, stop, pause, kill)
> - Container inspection and debugging
> - Image management commands
> - Cleaning up unused resources

[[Docker 3 - Containers & CLI]]

---

## Docker Hub & Registry
[[Docker 4 - Docker Hub & Registry]]

> [!summary] Topics Covered:
> - Docker Hub overview
> - Image categories (Official, Verified Publisher, Community)
> - Docker Hardened Images (DHI)
> - Filtering images from CLI
> - Choosing the right image

[[Docker 4 - Docker Hub & Registry]]

---

## Docker Compose
[[Docker 5 - Docker Compose]]

> [!summary] Topics Covered:
> - Docker Compose overview
> - compose.yaml structure
> - Service configuration
> - CLI commands
> - Real-world examples

[[Docker 5 - Docker Compose]]

---

## Volumes & Storage
[[Docker 6 - Volumes & Storage]]

> [!summary] Topics Covered:
> - Volume types (Named, Anonymous, Bind Mounts)
> - Persistent data storage
> - Real-world examples (PostgreSQL, WordPress)
> - Backup and restore
> - Sharing data between containers

[[Docker 6 - Volumes & Storage]]

---

## Networking
[[Docker 7 - Networking]]

> [!summary] Topics Covered:
> - Network types (Bridge, Host, User-Defined, Macvlan, IPvlan)
> - Port mapping
> - Service discovery by name
> - Network commands
> - Multi-host networking

[[Docker 7 - Networking]]

---

## Key Takeaways

- **Images** = blueprints; **Containers** = running instances
- **Layers** enable caching and space efficiency
- **Volumes** persist data beyond container lifecycle
- **Bridge** = default; **User-Defined** = DNS-based discovery
- **Docker Compose** orchestrates multi-container apps
- For production at scale → [[Kubernetes]]

---

## Common Commands Quick Reference

| Command | Description |
|---------|-------------|
| `docker run -d -p 8080:80 nginx` | Run container in background with port mapping |
| `docker ps` | List running containers |
| `docker stop <id>` | Stop container gracefully |
| `docker logs -f <id>` | View logs in real-time |
| `docker exec -it <id> bash` | Enter running container |
| `docker build -t name .` | Build image from Dockerfile |
| `docker compose up -d` | Start all services |
| `docker volume create name` | Create named volume |
| `docker network create name` | Create custom network |
