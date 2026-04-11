---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - DockerCompose
  - YAML
source: Docker Docs, GeeksforGeeks
---

# Docker: Docker Compose

## What is Docker Compose?

**Docker Compose** is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services, then create and start all the services from that configuration with a single command.

---

> [!tip] Why Use Docker Compose?
> - Manages multiple containers as a single application
> - Eliminates the need for long shell scripts
> - Easy to reproduce environments across machines
> - Perfect for: microservices, full-stack apps, CI/CD, development environments

---

## The Compose Application Model

The Compose Specification defines the components that make up an application:

| Component | Description |
|-----------|-------------|
| **Services** | Computing components (containers) that run specific images |
| **Networks** | Communication channels between containers |
| **Volumes** | Persistent storage shared between services |
| **Configs** | Runtime configuration files mounted into containers |
| **Secrets** | Sensitive data (passwords, keys) with security considerations |
| **Project** | Grouping unit for resources (defaults to directory name) |

---

## The Compose File

The `compose.yaml` file (formerly `docker-compose.yml`) is the heart of Docker Compose.

> [!note] Note on Version
> Modern Docker Compose no longer requires the `version` field. It's optional and deprecated in recent versions.

---

### Core Structure

A standard Compose file has three main top-level keys:

```yaml
services:     # Mandatory - defines containers
  web:
    image: nginx

volumes:      # Optional - defines persistent storage
  data:

networks:     # Optional - defines custom networks
  frontend:
```

---

### YAML Syntax Rules

| Rule | Description |
|------|-------------|
| **Indentation** | Must use spaces (standard: 2 spaces), never tabs |
| **Key-Value** | Format: `key: value` |
| **Lists** | Denoted by hyphen followed by space (`- item`) |
| **Strings** | Quotes optional, but recommended for special characters |

> [!warning] YAML Errors
> Misaligned indentation is the most common cause of Compose errors. Always verify spacing.

---

## Service Configuration Properties

The `services` block is where most configuration happens:

| Property | Description | Example |
|----------|-------------|---------|
| `image` | Image to pull from registry | `image: postgres:15` |
| `build` | Build from local Dockerfile | `build: ./backend` |
| `ports` | Map host ports to container ports | `- "8080:80"` |
| `environment` | Set environment variables | `DB_HOST: localhost` |
| `volumes` | Mount folders or named volumes | `- ./code:/app` |
| `depends_on` | Control startup order | `depends_on: - db` |
| `restart` | Restart policy | `restart: always` |
| `networks` | Connect to specific networks | `networks: - frontend` |
| `command` | Override default command | `command: npm start` |
| `env_file` | Load environment from file | `env_file: .env` |

---

### Port Mapping

Format: `"HOST_PORT:CONTAINER_PORT"`

```yaml
services:
  web:
    ports:
      - "8080:80"     # localhost:8080 → container port 80
      - "3000:3000"
```

---

### Environment Variables

#### Inline

```yaml
services:
  db:
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret123
      POSTGRES_DB: myapp
```

#### From File (.env)

Create `.env` file:
```
POSTGRES_VERSION=14.1
DB_PASSWORD=secret123
```

In `compose.yaml`:
```yaml
services:
  db:
    image: "postgres:${POSTGRES_VERSION}"
    environment:
      POSTGRES_PASSWORD: "${DB_PASSWORD}"
```

Compose automatically loads `.env` from the same directory.

---

### Volumes

```yaml
services:
  db:
    volumes:
      - db-data:/var/lib/postgresql/data    # Named volume
      - ./app:/app                          # Bind mount

volumes:
  db-data:    # Define the named volume
```

---

### Depends_on

Controls startup order:

```yaml
services:
  web:
    depends_on:
      - db
      - redis
```

> [!note] Limitation
> `depends_on` waits for the container to start, NOT for the application inside to be ready. Use healthchecks for that.

---

## Complete Example: Web App with Database

```yaml
services:
  # Frontend Web Server
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code              # Live code sync for development
    environment:
      FLASK_ENV: development
    depends_on:
      - redis
      - db

  # Redis Cache
  redis:
    image: redis:alpine
    volumes:
      - redis-data:/data

  # PostgreSQL Database
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: myapp
    volumes:
      - db-data:/var/lib/postgresql/data
    restart: always

volumes:
  redis-data:
  db-data:

networks:
  default:
    driver: bridge
```

---

## Example: WordPress with MySQL

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
    depends_on:
      - db

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

---

## Docker Compose CLI Commands

| Command | Description |
|---------|-------------|
| `docker compose up` | Create and start all services |
| `docker compose up -d` | Start in detached mode (background) |
| `docker compose down` | Stop and remove containers, networks |
| `docker compose logs` | View output from services |
| `docker compose logs -f` | Follow log output (real-time) |
| `docker compose ps` | List all services and their status |
| `docker compose stop` | Stop services |
| `docker compose start` | Start services |
| `docker compose restart` | Restart services |
| `docker compose build` | Rebuild images |
| `docker compose exec` | Execute command in running container |
| `docker compose pull` | Pull all images |
| `docker compose config` | Validate and view the final configuration |

---

### Common Command Patterns

#### Start Application

```bash
docker compose up -d
```

#### View Logs

```bash
docker compose logs -f
```

#### Stop Everything (Keep Data)

```bash
docker compose down
```

#### Stop and Delete Volumes (WARNING: Destroys Data)

```bash
docker compose down -v
```

#### Rebuild After Dockerfile Changes

```bash
docker compose build
docker compose up -d
```

#### Scale a Service

```bash
docker compose up -d --scale web=3
```

---

## Pro Tips

> [!tip] Docker Compose Automatically Creates a Network
> When you run `docker compose up`, Compose automatically creates a network for your services. Containers can communicate using service names as hostnames:
> ```yaml
> services:
>   web:
>     depends_on:
>       - db
>   # In web, connect to db using hostname: "db"
> ```

---

> [!tip] Use .dockerignore
> When using `build:` in Compose, place a `.dockerignore` file in your build context to exclude unnecessary files and speed up builds.

---

> [!tip] Multiple Compose Files
> Override settings for different environments:
> ```bash
> docker compose -f docker-compose.yml -f docker-compose.prod.yml up
> ```

---

## Docker Compose vs Kubernetes

| Aspect | Docker Compose | Kubernetes |
|--------|----------------|------------|
| **Scale** | Single host | Multiple hosts (cluster) |
| **Complexity** | Simple | Complex |
| **Use Case** | Development, simple deployments | Production, large-scale |
| **Load Balancing** | Manual | Built-in |
| **Self-healing** | Limited | Advanced |

---

## Key Takeaways

- **Docker Compose** = tool for multi-container application definition
- Uses **YAML** file (`compose.yaml`) to define services, networks, volumes
- `docker compose up -d` starts everything in background
- `docker compose down` stops and removes containers
- **Service names** act as hostnames for inter-container communication
- Perfect for development, testing, and simple production deployments
- For complex, multi-host production: use [[Kubernetes]]

---

## Related Notes

- [[Docker 1 - Introduction & Architecture]]
- [[Docker 2 - Images & Dockerfiles]]
- [[Docker 3 - Containers & CLI]]
- [[Docker 6 - Volumes & Storage]]
- [[Docker 7 - Networking]]
- [[YAML]]
- [[Kubernetes]]
- [[CI-CD]]