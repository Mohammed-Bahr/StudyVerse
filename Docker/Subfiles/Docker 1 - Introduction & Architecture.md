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

# Docker: Introduction & Architecture

## What is Docker?

**Docker** is a tool that allows you to **package an application and everything it needs** (code, libraries, dependencies, configuration) into a single unit called a **`container`**.

This container can run **the same way on any machine** that has Docker installed.

---

## What Problems Did Docker Solve?

### 1. "It works on my machine" Problem

**Before Docker:**
- An app works on the developer's laptop
- Fails on another laptop or the server
- Different OS, libraries, versions, configs

**Docker Solution:**
- The app and its environment are packaged together
- Runs the same everywhere

---

### 2. Dependency Conflicts

**Before Docker:**
- App A needs Python 3.8
- App B needs Python 3.11
- Installing one breaks the other

**Docker Solution:**
- Each app runs in its **own container**
- Each container has its own dependencies

---

### 3. Slow and Inconsistent Setup

**Before Docker:**
- Long setup guides
- Manual installs
- Easy to make mistakes

**Docker Solution:**
- One command: `docker run`
- Same setup for everyone

---

### 4. Heavy Virtual Machines

**Before Docker:**
- Virtual Machines include a full OS
- Large size
- Slow startup

**Docker Solution:**
- Containers share the host OS kernel
- Lightweight
- Start in seconds

---

## Core Concepts: Standardization & Isolation

### Standardization

Docker packages software so it runs exactly the same way on a developer's laptop, a testing server, or a cloud production environment.

**Means:** Running applications in a consistent, predictable environment everywhere.

**With Docker:**
- Same OS base image
- Same dependencies
- Same configuration
- Same behavior

> [!example] Example
> If it runs in Docker on your laptop, it will run the same on the server or cloud.

---

### Isolation

Docker keeps applications separate from each other, so a problem in one doesn't crash the other.

**Means:** Each application runs separately and cannot affect others.

**With Docker:**
- Each container has:
  - Its own **file system**
  - Its own **processes**
  - Its own **network** (optional)
- One container crashing **does not break** others

> [!example] Example
> App A crashes → App B keeps running normally.

---

> [!important] Key Distinction
> - **Container** = running instance of an image
> - **Image** = container in non-running mode (blueprint)

---

## Docker Architecture

![[DevOps/5. Docker/Attachments/image.png|image.png]]

### Key Components

| Component | Description |
|-----------|-------------|
| **Docker Engine** | Core system that creates and manages containers |
| **Docker Image** | Read-only template used to create containers |
| **Docker Hub** | Cloud-based repository for finding and sharing images |
| **Dockerfile** | Text file describing steps to create an image |
| **Docker Registry** | Storage distribution system for Docker images |

---

### Communication Protocols

> [!important] Protocol Usage
> 1. **IPC Protocol** → Communication between client and host on the **same device**
> 2. **REST Protocol** → Communication between client and host on **different devices**

---

## Docker Engine

Docker Engine is the main system that makes Docker work on your computer. It is responsible for creating, running, and managing containers.

Docker Engine uses a **client-server architecture**:

### 1. Docker Daemon (`dockerd`)

The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects:
- **Images** — templates used to create containers
- **Containers** — running instances of images
- **Networks** — how containers communicate
- **Volumes** — persistent storage for containers

> [!important]
> When you ask Docker to run a container, the daemon is the component that pulls the image, creates the container, and starts it.

---

### 2. Docker Client (CLI)

The Docker client is the tool you interact with directly using commands like:

```bash
docker run
docker build
docker ps
```

The client does **not** run containers itself. Instead, it sends commands to the Docker daemon using a **REST API**.

---

### How They Work Together

When you type a Docker command:

1. The **Docker client** sends the request to the **Docker daemon**
2. The **daemon** processes the request
3. The result is sent back to the client and shown to you

> [!tip] In Simple Terms
> - **Docker Client** → gives instructions
> - **Docker Daemon** → does the work
> - **Docker Engine** → the system that connects and manages everything

---

## How Container Isolation Works

Container isolation happens through several Linux kernel features:

### Key Isolation Mechanisms

| Mechanism | Function |
|-----------|----------|
| **Namespaces** | Create isolated views of system resources |
| **Control Groups (cgroups)** | Limit and track resource usage |
| **Union Filesystem** | Layered filesystem with copy-on-write |

---

### Namespaces

These create isolated views of system resources. Each container gets its own:

| Namespace | Isolation |
|-----------|-----------|
| **PID** | Process isolation — container sees only its own processes |
| **Network** | Separate network stack, IP addresses, routing tables |
| **Mount** | Isolated filesystem view |
| **UTS** | Separate hostname |
| **IPC** | Isolated inter-process communication |
| **User** | Map container users to different host users |

---

### Control Groups (cgroups)

These limit and track resource usage:
- CPU allocation
- Memory limits
- Disk I/O
- Network bandwidth
- Prevents one container from consuming all host resources

---

### Filesystem Isolation

Containers use layered filesystems like OverlayFS:
- Each container has its own root filesystem
- Copy-on-write layers keep changes separate
- Base images are shared read-only

---

### Network Isolation

Containers get virtual network interfaces:
- Bridge networks connect containers
- Port mapping exposes services selectively
- Network policies control traffic between containers

---

## Containers vs Virtual Machines (VMs)

Both containers and VMs isolate applications, but they do it in fundamentally different ways.

![](https://media2.dev.to/dynamic/image/width=1000,height=500,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9zk0491ay21mp2tyea79.png)

---

### Architecture Comparison

| Aspect | Virtual Machines | Containers |
|--------|-----------------|------------|
| **Type** | Hardware Virtualization | OS Virtualization |
| **Requires** | Hypervisor (VMware, VirtualBox) | Container Engine (Docker) |
| **OS** | Each VM has full Guest OS | Share Host OS kernel |
| **Contains** | Full OS + App + Dependencies | App + Dependencies only |

---

### Key Differences at a Glance

| Feature | Virtual Machine (VM) | Docker Container |
|---------|---------------------|------------------|
| **Weight** | Heavyweight: Full OS (GBs) | Lightweight: Shares host OS (MBs) |
| **Startup Time** | Slow: Minutes (boots entire OS) | Fast: Milliseconds/Seconds |
| **Resource Usage** | High: Pre-allocates RAM and CPU | Low: Uses resources dynamically |
| **Isolation** | Strong: Hardware level | Process level (sufficient for most apps) |
| **Portability** | Lower: Bulky to move | High: "Write once, run anywhere" |

---

### Summary Analogy

> [!tip] House vs Apartment
> - **Virtual Machine** = **House**
>   - Fully self-contained, own infrastructure, plumbing, heating
>   - Takes up lots of space and resources
>   
> - **Container** = **Apartment**
>   - Private space (your app)
>   - Shares foundation, plumbing, heating (OS Kernel) with neighbors
>   - Much more efficient

---

### Isolation Depth

**Containers:**
- Share the **same OS kernel**
- Isolation done by OS (namespaces & cgroups)
- Lighter and faster
- **Weaker isolation** than VMs (but good enough for most apps)

> **Containers are like rooms in the same house** — separated, but share the same building.

**Virtual Machines:**
- Each VM has its **own full OS**
- Isolation done by a **hypervisor**
- Heavier and slower
- **Stronger isolation** (more secure)

> **VMs are like separate houses** — each fully independent.

---

> [!important] One-Line Summary
> - **Containers**: Fast, lightweight, less isolation
> - **VMs**: Slower, heavy, very strong isolation

---

## Key Takeaways

- Docker packages applications with all dependencies into portable **containers**
- Solves: "Works on my machine", dependency conflicts, slow setup, heavy VMs
- Uses **Linux namespaces**, **cgroups**, and **union filesystem** for isolation
- **Docker Engine** = Client (CLI) + Daemon (server)
- **Containers share OS kernel** → lightweight; **VMs have full OS** → isolated but heavy
- For most applications, containers provide sufficient isolation with better efficiency

---

## Related Notes

- [[Docker 2 - Images & Dockerfiles]]
- [[Docker 3 - Containers & CLI]]
- [[Docker 6 - Volumes & Storage]]
- [[Docker 7 - Networking]]
- [[Linux Namespaces]]
- [[cgroups]]
- [[Kubernetes]] — Container orchestration at scale