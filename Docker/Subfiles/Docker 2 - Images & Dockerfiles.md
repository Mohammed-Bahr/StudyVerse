---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - Containers
  - Dockerfile
source: Docker Docs, GeeksforGeeks
---

# Docker: Images & Dockerfiles

## What is a Docker Image?

A **Docker image** is a **read-only template** that contains everything needed to run an application inside a container. It acts as the **blueprint** for creating containers.

---

### What a Docker Image Contains

- **Application code**
- **Runtime environment** (e.g., Node.js, Python, Java)
- **System libraries and dependencies**
- **Configuration files**
- Default commands to run the app

> [!important] Key Point
> Because all dependencies are packaged inside the image, the application runs **the same way on any machine**.

---

## Image vs Container

| Concept | State | Analogy |
|---------|-------|---------|
| **Docker Image** | Static, read-only, not running | Class / Blueprint |
| **Docker Container** | Running instance of an image | Object created from class |

You can create many containers from the same image.

---

## Image Layers (Key Concept)

Docker images are made of **layers**:

- Each Dockerfile instruction = **one layer**
- Layers are **cached and reused**
- Multiple images can **share the same layers**

This makes Docker:
- Faster to build
- More storage-efficient

---

### Layers of a Docker Image

| Layer | Description |
|-------|-------------|
| **1. Base Layer** | Starting point (e.g., `ubuntu`, `alpine`, `node`). Provides OS environment and basic libraries. |
| **2. Intermediate Layers** | Created by each Dockerfile instruction (`RUN`, `COPY`, `ADD`). Contains installed packages, copied files, dependencies. Cached and reused. |
| **3. Final Layer Stack** | Combination of all previous layers. The final image. |
| **4. Container Writable Layer** | Added at runtime. All file changes happen here. Image layers below remain read-only. |

---

## How Docker Optimizes Space

### Layer Reuse
- If multiple images use the same base image or instructions, Docker stores that layer only once
- Example: Many images can share the same `ubuntu` base layer

### Caching During Builds
- Docker caches layers
- If a Dockerfile instruction hasn't changed, Docker reuses the cached layer instead of rebuilding
- Saves disk space and build time

### Copy-on-Write (CoW)
- Image layers are read-only
- When a container modifies a file, Docker copies it into the writable layer
- Avoids duplicating entire layers

---

## How Docker Optimizes Performance

- **Faster Image Builds** — Cached layers allow Docker to skip unchanged steps
- **Faster Container Startup** — Containers start quickly because they reuse pre-built layers
- **Efficient Distribution** — Docker downloads only layers that don't already exist locally

---

## What is a Dockerfile?

A **Dockerfile** is a simple text file containing instructions in a **Domain-Specific Language (DSL)** that tell Docker how to build an image step by step.

Think of a Dockerfile as a **recipe**:
- Each line is an instruction (like `FROM`, `RUN`, `COPY`, `CMD`)
- Each instruction adds a new **layer** to the Docker image
- When all instructions finish, the result is a **Docker image**
- Executed **top to bottom** by the Docker daemon

Because of this, the Dockerfile is often called **the source code of a Docker image**.

---

## Execution Order

The Docker daemon reads and executes the Dockerfile **from top to bottom**:

1. Starts with a base image
2. Applies each instruction in order
3. Each step builds on the result of the previous one

> [!warning] Order Matters!
> - Changing an early line may force Docker to rebuild everything after it
> - Proper ordering improves build speed using Docker's cache
> - **Best Practice:** Put frequently changing instructions (like `COPY . .`) at the end

---

## Dockerfile Instructions Reference

| Instruction | Purpose | Example |
|-------------|---------|---------|
| `FROM` | Sets the base image (must be first) | `FROM ubuntu:22.04` |
| `RUN` | Executes commands during build | `RUN apt-get update && apt-get install -y curl` |
| `COPY` | Copies files from host to image | `COPY . /app` |
| `ADD` | Like COPY but supports URLs & auto-extraction | `ADD https://example.com/file.tar.gz /app/` |
| `WORKDIR` | Sets working directory | `WORKDIR /app` |
| `ENV` | Sets environment variables | `ENV NODE_ENV=production` |
| `EXPOSE` | Documents ports the container listens on | `EXPOSE 8080` |
| `CMD` | Default command when container starts | `CMD ["node", "server.js"]` |
| `ENTRYPOINT` | Configures container as executable | `ENTRYPOINT ["python", "app.py"]` |
| `VOLUME` | Creates mount point for persistent data | `VOLUME /data` |
| `USER` | Sets user to run subsequent commands | `USER appuser` |
| `ARG` | Defines build-time variables | `ARG VERSION=1.0` |
| `LABEL` | Adds metadata to image | `LABEL maintainer="admin@example.com"` |
| `HEALTHCHECK` | Tells Docker how to test container health | `HEALTHCHECK CMD curl -f http://localhost/` |
| `ONBUILD` | Adds trigger for when image is used as base | `ONBUILD COPY . /app/src` |

---

## CMD vs ENTRYPOINT vs RUN

> [!important] Key Differences
> - `RUN` → Executes during **image build**
> - `CMD` → Executes when **container starts** (can be overridden)
> - `ENTRYPOINT` → Defines the **main executable** (harder to override)

---

### Detailed Comparison

| Aspect | RUN | CMD | ENTRYPOINT |
|--------|-----|-----|------------|
| **When** | Build time | Container start | Container start |
| **Purpose** | Install packages, run commands | Set default command | Define main executable |
| **Override** | N/A | Can be overridden by `docker run` | Arguments appended, not replaced |
| **Layers** | Creates new layer | No new layer | No new layer |

---

### Example: CMD vs ENTRYPOINT Behavior

**With CMD:**

```dockerfile
FROM ubuntu
CMD ["echo", "Hello"]
```

```bash
docker run myimage            # Output: Hello
docker run myimage "World"    # Output: World (CMD is replaced entirely)
```

**With ENTRYPOINT:**

```dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
CMD ["Hello"]
```

```bash
docker run myimage            # Output: Hello
docker run myimage "World"    # Output: World (argument appended to echo)
```

---

> [!tip] Best Practice
> Use `ENTRYPOINT` for the main executable and `CMD` for default arguments. This allows users to override arguments easily.

---

## COPY vs ADD

| Aspect | COPY | ADD |
|--------|------|-----|
| **Purpose** | Copy local files to container | Copy files, fetch URLs, extract archives |
| **Simple file copy** | ✅ Preferred | Works, but overkill |
| **URL fetching** | ❌ No | ✅ Yes |
| **Auto-extract tar** | ❌ No | ✅ Yes |
| **Recommended** | ✅ Use for local files | ⚠️ Only when you need special features |

> [!warning] Security Note
> Avoid `ADD` with URLs — it can introduce security risks and makes builds less reproducible. Use `RUN curl` instead for URL downloads.

---

## Multi-Stage Builds

Multi-stage builds allow you to use multiple `FROM` statements in a single Dockerfile. This produces smaller, more secure final images.

### Why Use Multi-Stage Builds?

- **Smaller final image** — Build tools stay in build stage, not in final image
- **Better security** — No compilers, package managers in production image
- **Cleaner separation** — Build-time vs runtime dependencies

---

### Example: Node.js Multi-Stage Build

```dockerfile
# Stage 1: Build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Production (smaller)
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
CMD ["node", "dist/server.js"]
```

**Result:** The final image is much smaller because it doesn't include build tools.

---

### Example: Go Application

```dockerfile
# Build stage
FROM golang:1.21 AS builder
WORKDIR /app
COPY go.* ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o myapp

# Minimal production stage
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

---

## .dockerignore

The `.dockerignore` file prevents unnecessary files from being copied into the image during build. This improves build speed and security.

### Example .dockerignore

```
# Dependencies
node_modules
npm-debug.log

# Version control
.git
.gitignore

# Environment files (SECURITY RISK!)
.env
.env.local

# IDE
.vscode
.idea

# Docker files
Dockerfile
docker-compose.yml
.dockerignore

# Documentation
README.md
docs/

# Tests
tests/
*.test.js
coverage/
```

> [!warning] Security Critical
> **Never** copy `.env` files into images! Use Docker secrets or environment variables at runtime.

---

## Dockerfile Best Practices

### 1. Layer Ordering for Cache Efficiency

```dockerfile
# ❌ Bad - Rebuilds npm install on every code change
COPY . /app
RUN npm install

# ✅ Good - Only rebuilds npm install if package.json changes
COPY package*.json ./
RUN npm install
COPY . .
```

---

### 2. Combine RUN Commands

```dockerfile
# ❌ Bad - Creates multiple layers
RUN apt-get update
RUN apt-get install -y python3
RUN apt-get install -y curl
RUN apt-get clean

# ✅ Good - Single layer
RUN apt-get update && apt-get install -y \
    python3 \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
```

---

### 3. Use Specific Base Image Tags

```dockerfile
# ❌ Bad - Unpredictable
FROM node

# ✅ Good - Reproducible
FROM node:18.19-alpine
```

---

### 4. Never Run as Root

```dockerfile
# Create non-root user
RUN useradd -m -s /bin/bash appuser
USER appuser
```

---

### 5. Use Minimal Base Images

| Image | Size | Use Case |
|-------|------|----------|
| `ubuntu:22.04` | ~77MB | Full Linux environment |
| `alpine` | ~5MB | Minimal, security-focused |
| `node:18-alpine` | ~50MB | Node.js apps |
| `distroless/*` | ~20MB | Ultra-minimal, production |

---

> [!warning] Security Best Practices
> - Never run containers as root (`USER` directive)
> - Use minimal base images (`alpine`, `distroless`)
> - Scan images for vulnerabilities (`docker scout`)
> - Don't store secrets in image layers — use secrets/volumes
> - Use `.dockerignore` to prevent leaking sensitive files
> - Pin image versions for reproducibility

---

## Complete Example Dockerfile

```dockerfile
# Use specific version of Node.js on Alpine Linux
FROM node:18.19-alpine

# Set working directory
WORKDIR /app

# Copy package files first (for caching)
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Create non-root user for security
RUN addgroup -g 1001 -S nodejs \
    && adduser -S nextjs -u 1001

# Switch to non-root user
USER nextjs

# Document the port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
    CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

# Set metadata
LABEL version="1.0.0"
LABEL maintainer="dev@example.com"

# Start the application
CMD ["node", "server.js"]
```

---

## Key Takeaways

- **Images** = read-only blueprints; **Containers** = running instances
- Images use **layers** for space efficiency and caching
- Each Dockerfile instruction creates a new layer
- **Layer ordering matters** — put frequently changing instructions last
- Use `COPY` over `ADD`; prefer `CMD` for defaults, `ENTRYPOINT` for executables
- **Multi-stage builds** produce smaller, more secure images
- Always use `.dockerignore` to control what enters the build context
- Follow security best practices: non-root user, minimal images, version pinning

---

## Related Notes

- [[Docker 1 - Introduction & Architecture]]
- [[Docker 3 - Containers & CLI]]
- [[Docker 4 - Docker Hub & Registry]]
- [[Docker 6 - Volumes & Storage]]
- [[YAML]]
- [[CI-CD]]
- [[Node.js]]
- [[Linux Package Management]]