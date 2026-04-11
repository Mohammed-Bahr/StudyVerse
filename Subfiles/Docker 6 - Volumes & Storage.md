---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - Volumes
  - Storage
  - Persistence
source: Docker Docs, GeeksforGeeks
---

# Docker: Volumes & Storage

## What are Docker Volumes?

**Docker volumes** are the primary mechanism for **persistent data storage** in containerized applications. When a container is deleted, any data written to its writable layer is lost. Volumes solve this by mounting external storage into the container filesystem.

> [!important] Why Volumes Matter
> Volumes are essential for stateful applications in production, separating data lifecycle from application deployment.

---

## Core Concepts

| Aspect | Description |
|--------|-------------|
| **Purpose** | Persist data beyond container lifecycle |
| **Location** | Managed by Docker (`/var/lib/docker/volumes/` on Linux) |
| **Sharing** | Multiple containers can mount the same volume |
| **Performance** | Bypasses copy-on-write; native filesystem speed |

---

## Volume Types

### 1. Named Volumes (Recommended)

Docker-managed storage with a specific name.

```bash
# Create explicitly
docker volume create my-data

# Use in container
docker run -d -v my-data:/app/data --name myContainer my-image
```

> [!tip] Best Practice
> Named volumes are the preferred way to persist data in production.

---

### 2. Anonymous Volumes

Created without a name; Docker assigns a random ID.

```bash
docker run -v /app/data my-image
```

**Use case:** Temporary cache directories you don't need to reference later.

---

### 3. Bind Mounts

Mount a specific host directory or file into the container.

```bash
# Old syntax
docker run -v /host/path:/container/path my-image

# Modern syntax (preferred)
docker run --mount type=bind,source=/host/path,target=/container/path my-image
```

**Use case:** Development — live code syncing between host and container.

---

## Volume Syntax Comparison

| Type | Syntax | Example |
|------|--------|---------|
| Named Volume | `-v name:path` | `-v my-data:/app/data` |
| Bind Mount | `-v /host/path:/container/path` | `-v ./src:/app/src` |
| Anonymous | `-v /path` | `-v /app/temp` |

---

## Key Commands

| Command | Description |
|---------|-------------|
| `docker volume create my-volume` | Create a named volume |
| `docker volume ls` | List all volumes |
| `docker volume inspect my-volume` | Show volume details |
| `docker volume rm my-volume` | Remove a volume |
| `docker volume prune` | Remove all unused volumes |
| `docker system df -v` | See volume disk usage |

---

> [!important] Multiple Containers, Same Volume
> You can share a volume between multiple containers, or mount multiple volumes in one container.

---

## Real Example 1: PostgreSQL with Persistent Storage

### Step 1: Create the Volume

```bash
docker volume create postgres_data
```

### Step 2: Run PostgreSQL with Volume

```bash
docker run -d \
  --name my-postgres \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=mysecretpassword \
  -e POSTGRES_DB=myapp \
  -v postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15
```

### Step 3: Verify and Test

```bash
# Check volume exists
docker volume ls

# Inspect storage location
docker volume inspect postgres_data

# Connect and create data
docker exec -it my-postgres psql -U admin -d myapp -c "CREATE TABLE users (id serial PRIMARY KEY, name varchar(50));"
docker exec -it my-postgres psql -U admin -d myapp -c "INSERT INTO users (name) VALUES ('Alice'), ('Bob');"
```

### Step 4: Test Persistence

```bash
# Stop and remove container
docker stop my-postgres
docker rm my-postgres

# Start new container with SAME volume
docker run -d \
  --name my-postgres-new \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=mysecretpassword \
  -e POSTGRES_DB=myapp \
  -v postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15

# Data still exists!
docker exec -it my-postgres-new psql -U admin -d myapp -c "SELECT * FROM users;"
```

---

## Real Example 2: Development with Live Code (Bind Mount)

```bash
# Create project directory
mkdir ~/my-webapp && cd ~/my-webapp
echo "<h1>Hello from Docker!</h1>" > index.html

# Run nginx with bind mount
docker run -d \
  --name dev-nginx \
  -v ~/my-webapp:/usr/share/nginx/html:ro \
  -p 8080:80 \
  nginx:alpine

# Open http://localhost:8080 in browser

# Edit file on host
echo "<h1>Updated from host!</h1>" > index.html

# Refresh browser — changes appear immediately (no rebuild needed)
```

---

## Real Example 3: Multi-Container with Docker Compose

```yaml
services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress_data:/var/www/html
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    volumes:
      - db_data:/var/lib/mysql

volumes:
  wordpress_data:
  db_data:
```

```bash
# Start everything
docker compose up -d

# Stop (data remains)
docker compose down

# Stop and delete volumes (WARNING: destroys data)
docker compose down -v
```

---

## Real Example 4: Backup & Restore Volume Data

### Backup

```bash
docker run --rm \
  -v postgres_data:/source:ro \
  -v $(pwd):/backup \
  busybox \
  tar czf /backup/postgres_backup.tar.gz -C /source .
```

### Restore

```bash
# Create new volume
docker volume create postgres_data_new

# Restore data
docker run --rm \
  -v postgres_data_new:/target \
  -v $(pwd):/backup \
  busybox \
  tar xzf /backup/postgres_backup.tar.gz -C /target
```

---

## Real Example 5: Sharing Data Between Containers

```bash
# Create shared volume
docker volume create shared_logs

# Container 1: Write logs
docker run -d \
  --name logger \
  -v shared_logs:/app/logs \
  busybox \
  sh -c "while true; do echo $(date) >> /app/logs/app.log; sleep 5; done"

# Container 2: Read logs (running simultaneously)
docker run -d \
  --name log-processor \
  -v shared_logs:/data:ro \
  busybox \
  sh -c "tail -f /data/app.log"

# View shared data
docker exec log-processor cat /data/app.log
```

---

## When to Use What

| Scenario | Solution |
|----------|----------|
| Database persistence | Named volume |
| Development code syncing | Bind mount |
| Temporary/cache data | Anonymous volume or tmpfs |
| Multi-host clustering | Volume driver (NFS, cloud) |
| Secrets/config files | Bind mount (read-only) |

---

## Quick Reference: Common Patterns

| Task | Command |
|------|---------|
| See volume disk usage | `docker system df -v` |
| Copy files into volume | `docker cp file.txt container:/path/` |
| Clean up unused volumes | `docker volume prune` |
| Mount volume read-only | `-v my-vol:/path:ro` |
| Use tmpfs (in-memory) | `--tmpfs /path` |

---

## Important Considerations

### Permissions

Container users need correct UID/GID for volume access:

```dockerfile
# Fix permissions in Dockerfile
RUN chown -R appuser:appuser /app/data
USER appuser
```

---

### Volume Drivers

For advanced use cases (cloud storage, NFS):

```bash
docker volume create --driver nfs my-nfs-volume
```

---

### tmpfs Mounts

For sensitive data that should never touch disk:

```bash
docker run --tmpfs /tmp:rw,size=100m my-image
```

---

## Key Takeaways

- **Volumes persist data** beyond container lifecycle
- **Named volumes** = Docker-managed (production)
- **Bind mounts** = Host-managed (development)
- **Anonymous volumes** = Temporary data
- Multiple containers can **share volumes**
- Always backup critical volumes
- Use `docker volume prune` to clean up unused volumes

---

## Related Notes

- [[Docker 1 - Introduction & Architecture]]
- [[Docker 2 - Images & Dockerfiles]]
- [[Docker 3 - Containers & CLI]]
- [[Docker 5 - Docker Compose]]
- [[PostgreSQL]] — Common database for containers
- [[Linux File Permissions]] — UID/GID for volume access
- [[Storage Systems]]