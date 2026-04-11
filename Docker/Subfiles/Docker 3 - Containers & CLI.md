---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - Containers
  - CLI
source: Docker Docs, GeeksforGeeks
---

# Docker: Containers & CLI Commands

## Docker Container Lifecycle

The Docker container lifecycle consists of five primary states:

1. **Created** — Initial state, container instantiated but not started
2. **Running** — Container is executing its main process
3. **Paused** — Processes frozen, but resources remain intact
4. **Stopped (Exited)** — Main process terminated, data preserved
5. **Deleted (Killed)** — Container permanently removed

---

### Lifecycle States Explained

| State | Description | Docker Command |
|-------|-------------|----------------|
| **Created** | Container's filesystem and configuration prepared, but no process running | `docker create` |
| **Running** | Container executing main process, resources (CPU, memory, network) allocated | `docker start` or `docker run` |
| **Paused** | Processes frozen using cgroup freezer, can be unpaused to resume | `docker pause` |
| **Stopped** | Main process terminated gracefully or due to error, data logs preserved | `docker stop` |
| **Deleted** | Container permanently removed, writable layer freed | `docker rm` |

![](https://k21academy.com/wp-content/uploads/2020/11/DockerLifecycle_BlogImage-1024x573.png)

---

## Starting Docker Containers

### Option 1: Build from Dockerfile

```bash
docker build -t ImageName Path
```

### Option 2: Pull from Docker Hub

```bash
docker pull ProjectName
```

### Option 3: Create from Image (then start)

```bash
# Create container (not running)
docker container create ImageName

# Start the created container
docker container start container_id_or_name
```

> [!note] Note
> `docker create` only creates the container — it doesn't start a running process. You must manually call `docker start`.

---

## docker run vs docker start

| Command | Purpose | Works On |
|---------|---------|----------|
| `docker run` | Create AND start a container | Images |
| `docker start` | Start a stopped/paused container | Existing containers |

---

## Docker CLI: Container Management

### Basic Syntax

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

### Common Examples

#### Interactive Mode

```bash
docker run -it ubuntu /bin/bash
```

- `-i` — Interactive (keeps STDIN open)
- `-t` — Allocates a pseudo-TTY (terminal)

**Use case:** Debugging or exploring inside a container

---

#### Detached Mode (Background)

```bash
docker run -d -p 8080:80 --name my-web-server nginx
```

- `-d` — Detached mode (runs in background)
- `-p` — Port mapping (`host_port:container_port`)
- `--name` — Assigns a custom name

**Use case:** Running web servers, databases, APIs

---

#### Named Container

```bash
docker run -d --name ContainerName imageName
```

---

## Container Management Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker run` | Create and start a new container | `docker run -d -p 80:80 nginx` |
| `docker ps` | List running containers | `docker ps` |
| `docker ps -a` | List all containers (running + stopped) | `docker ps -a` |
| `docker start` | Start a stopped container | `docker start container_id` |
| `docker stop` | Stop a running container gracefully | `docker stop container_id` |
| `docker kill` | Force stop a container immediately | `docker kill container_id` |
| `docker rm` | Remove a container | `docker rm container_id` |
| `docker exec` | Run command inside running container | `docker exec -it container_id bash` |
| `docker logs` | View container logs | `docker logs -f container_id` |
| `docker inspect` | Display detailed container info | `docker inspect container_id` |
| `docker stats` | Live stream of resource usage (CPU, memory, I/O) | `docker stats my_container` |
| `docker container prune` | Remove all stopped containers | `docker container prune` |

---

> [!tip] New vs Old Syntax
> You may see both `docker ---` and `docker container ---`.  
> `docker container ---` is the newer, more explicit syntax. Both work.

---

## Stop vs Pause vs Kill

### docker stop

**What it does:**
- Sends **SIGTERM** signal
- Gives the app time to shut down gracefully
- After timeout (default 10 seconds), sends **SIGKILL**

**Use when:**
- You want to safely stop the container
- The app needs to save data, close connections, or clean up

**To continue:**
```bash
docker start <container_name_or_id>
```

**Data:** Container data is preserved. Volumes are NOT affected.

---

### docker kill

**What it does:**
- Immediately sends **SIGKILL**
- Container stops instantly
- No cleanup, no graceful shutdown

**Use when:**
- The container is frozen or not responding
- You need it stopped right now

**To continue:**
```bash
docker start <container_name_or_id>
```

> [!warning] Risk
> The app may lose unsaved data. Can corrupt files if the app was writing data.

---

### docker pause

**What it does:**
- Freezes all processes using **SIGSTOP**
- Container state is kept in memory
- No CPU usage while paused

**Use when:**
- You want to temporarily freeze the container
- Useful for debugging or freeing CPU

**To continue:**
```bash
docker unpause <container_name_or_id>
```

**Data:** Nothing is lost. App resumes from the exact same point.

---

### Quick Comparison

| Command | Graceful | Immediate | Resume Method | Typical Use |
|---------|----------|-----------|---------------|-------------|
| `docker stop` | ✅ Yes | ❌ No | `docker start` | Normal shutdown |
| `docker kill` | ❌ No | ✅ Yes | `docker start` | Emergency stop |
| `docker pause` | — | — | `docker unpause` | Temporary freeze |

---

## Inspecting & Debugging Containers

### View Logs

```bash
docker logs <container_name_or_id>
```

Add `-f` to follow logs in real-time (like `tail -f`):
```bash
docker logs -f <container_name_or_id>
```

---

### Execute Commands Inside Running Container

```bash
docker exec -it <container_name_or_id> /bin/bash
```

> [!note] Note
> `-it` = interactive terminal. This takes you directly to the container shell.

---

### Inspect Metadata

```bash
docker inspect <container_name_or_id>
```

Returns JSON with:
- IP address
- Volumes
- Environment variables
- Network settings
- etc.

---

## Image Management Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker build` | Build image from Dockerfile | `docker build -t myapp:latest .` |
| `docker images` | List all local images | `docker images` |
| `docker pull` | Download image from registry | `docker pull ubuntu:22.04` |
| `docker push` | Upload image to registry | `docker push username/myapp:latest` |
| `docker rmi` | Remove an image | `docker rmi image_id` |
| `docker tag` | Tag an image | `docker tag old:tag new:tag` |
| `docker image ls` | List images (alias) | `docker image ls` |

---

## Network & Volume Commands

| Command | Description | Example |
|---------|-------------|---------|
| `docker network create` | Create a network | `docker network create my-network` |
| `docker network ls` | List networks | `docker network ls` |
| `docker network connect` | Connect container to network | `docker network connect net container` |
| `docker volume create` | Create a volume | `docker volume create my-volume` |
| `docker volume ls` | List volumes | `docker volume ls` |

---

## Cleaning Up

### Remove Container

```bash
# Container must be stopped first
docker rm <container_name_or_id>
```

### Force Remove Running Container

```bash
docker rm -f <container_name_or_id>
# or
docker rm <container_name_or_id> --force
```

### Remove All Stopped Containers

```bash
docker container prune
```

### Remove Unused Images

```bash
docker image prune
```

### Complete System Cleanup

```bash
docker system prune
```

> [!warning] Caution
> `docker system prune` removes all unused containers, networks, images, and build cache. Use carefully.

---

## Summary Table: Essential Commands

| Command | Description |
|---------|-------------|
| `docker run` | Create and start a container |
| `docker ps` | List running containers |
| `docker stop` | Gracefully stop a container |
| `docker start` | Start a stopped container |
| `docker rm` | Delete a container |
| `docker logs` | Show container output |
| `docker exec` | Run a command inside a running container |
| `docker inspect` | View detailed container information |
| `docker stats` | Monitor resource usage in real-time |

---

## Quick Reference: Common Patterns

| Task | Command |
|------|---------|
| Run container in background | `docker run -d --name myapp image` |
| Map ports | `docker run -p 8080:80 image` |
| Mount volume | `docker run -v vol_name:/path image` |
| Set environment variable | `docker run -e VAR=value image` |
| Use specific network | `docker run --network mynet image` |
| Limit memory | `docker run --memory=512m image` |
| Limit CPU | `docker run --cpus=1.5 image` |
| Run as specific user | `docker run --user 1000 image` |
| Auto-restart on failure | `docker run --restart unless-stopped image` |

---

## Key Takeaways

- **Container lifecycle**: Created → Running → Paused → Stopped → Deleted
- `docker run` = create + start; `docker start` = start existing container
- **Stop** = graceful (SIGTERM); **Kill** = immediate (SIGKILL); **Pause** = freeze (SIGSTOP)
- Use `docker exec -it` to enter a running container
- `docker logs -f` for real-time debugging
- `docker inspect` for detailed metadata
- Clean up with `docker container prune` or `docker system prune`

---

## Related Notes

- [[Docker 1 - Introduction & Architecture]]
- [[Docker 2 - Images & Dockerfiles]]
- [[Docker 5 - Docker Compose]]
- [[Docker 6 - Volumes & Storage]]
- [[Docker 7 - Networking]]
- [[Linux Process Signals]] — SIGTERM, SIGKILL, SIGSTOP
- [[Bash]] — Shell basics for container interaction