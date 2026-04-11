---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - Networking
  - Bridge
  - VLAN
source: Docker Docs, GeeksforGeeks
---


## Docker Networking Overview

Docker networking enables containers to communicate with each other, the host, and the outside world. Each container runs in an isolated network environment with its own IP and interfaces.

By default, containers on the same host can securely communicate with each other without exposing ports to the host.

---

## Core Networking Concepts

### Key Linux Features Used by Docker

| Feature | Description |
|---------|-------------|
| **Network Namespaces** | Provides network isolation. Each container gets its own network interfaces, IP addresses, and routing tables. |
| **Virtual Ethernet (veth pairs)** | Virtual cable linking container to host, usually through a virtual bridge |
| **iptables** | Controls traffic, enables port mapping and NAT for forwarding |

---

## Docker Network Types

Docker provides **7 types** of networking:

| Network Type | Description |
|--------------|-------------|
| **Bridge** | Default network for standalone containers |
| **Host** | Removes network isolation, shares host network |
| **Overlay** | Multi-host networking (Swarm) |
| **Macvlan** | Direct access to physical network |
| **None** | Disables all networking |
| **Network Plugins** | Third-party networking solutions |
| **User-Defined** | Custom bridge networks |

---

## 1. Bridge Networking (Default)

If you run a container without specifying a network mode, it uses **Bridge** mode. Think of this as creating a private, gated community inside your computer.

![[DevOps/5. Docker/Attachments/image 2.png]]

### How It Works

- Docker creates a virtual network called `docker0` on your host machine
- Each container connected to this bridge gets its own internal IP (e.g., `172.17.0.2`)
- The container is network-isolated — cannot be accessed from outside unless you explicitly open the gate

### Port Mapping

To allow external traffic, you must use **port mapping** (`-p` flag):

```bash
docker run -d -p 8080:80 nginx
```

This maps:
- **Host port:** 8080
- **Container port:** 80

### Key Characteristics

| Aspect | Description |
|--------|-------------|
| **IP Address** | Internal IP (e.g., `172.17.0.2`) |
| **Communication** | Containers on same bridge can talk; must use gateway to talk to host |
| **Performance** | Slight overhead due to NAT (Network Address Translation) |

---

### Connecting a Container to Another Network

A container can be connected to **more than one network**. When attached to two networks, Docker assigns **two different IP addresses** — one per network.

**Steps:**

1. Create networks:
   ```bash
   docker network create net1
   docker network create net2
   ```

2. Run container on first network:
   ```bash
   docker run -dit --name container1 --network net1 ubuntu
   ```

3. Connect to second network:
   ```bash
   docker network connect net2 container1
   ```

**Result:** `container1` now has two IPs — one in `net1`, one in `net2`

> [!tip] Key Point
> A container connected to multiple networks acts as a **bridge** between them.

---

## 2. User-Defined Networks

User-defined networks are custom-built isolated networks that allow specific containers to communicate securely and efficiently.

![[DevOps/5. Docker/Attachments/image 3.png]]

### Why Use User-Defined Networks?

#### 1. Containers Find Each Other by Name

- **Default bridge:** Containers communicate using IP addresses (IPs can change)
- **User-defined network:** Containers can communicate using **container names** (DNS-based service discovery)

#### 2. More Security and Control

- Default bridge: All containers can talk to each other
- User-defined: You control which containers can communicate

#### 3. Dynamic Connection

- Add/remove containers to networks **without stopping them**

---

### Quick Start Commands

```bash
# Create network
docker network create NetworkName

# Run container in network
docker run -d --name ContainerName --network NetworkName imageName

# Connect another container to same network
docker run -d --name AnotherContainer --network NetworkName myImageName

# Connect running container to network
docker network connect NetworkName ExistingContainer
```

> [!tip] Docker Compose
> If you're using **Docker Compose**, you're already using user-defined networks! Compose automatically creates a dedicated network for your services.

> [!note] Note
> When creating a user-defined network, containers can ping each other by name — but only if they're on the **same** user-defined network.

---

## 3. Host Networking

**Host** networking removes network isolation between the container and Docker host.

![[DevOps/5. Docker/Attachments/image 4.png]]

### How It Works

- Container shares the **exact same** networking namespace as the host
- Does **not** get its own IP — uses host's IP directly
- If container listens on port 80, it's essentially listening on port 80 of your actual computer

### No Port Mapping Needed

```bash
# No -p flag needed
docker run -d --name my-webserver --network host nginx
```

### Key Characteristics

| Aspect | Description |
|--------|-------------|
| **IP Address** | Shares Host's IP |
| **Port Conflicts** | Cannot run two containers on same port |
| **Performance** | Faster — no NAT or bridge overhead |
| **OS Limitation** | Works natively on Linux; different on Docker Desktop (Windows/Mac) |

> [!warning] Important Note
> Any container in host network can see and ping any other container in any network type — even if that container has no network. It can see but can't communicate.

---

## 4. Macvlan Networking

**Macvlan** allows each container to have a unique MAC address, making it appear as a **standalone physical device** on your network.

![[DevOps/5. Docker/Attachments/image 5.png]]

### Why Use Macvlan?

| Aspect | Bridge Mode | Macvlan Mode |
|--------|-------------|--------------|
| IP Address | Host's IP + port mapping | Each container gets **real LAN IP** |
| Port Mapping | Required | Not needed |
| Network Visibility | Behind host | Appears as physical device |

### Use Cases

- **Legacy apps** expecting to be directly on the network
- Need specific MAC address for licensing/security
- Applications requiring direct LAN access

---

### How to Use Macvlan

#### Step A: Find Your Network Interface

```bash
ip link show
```

Look for your physical network card (e.g., `eth0`, `ens33`, `wlan0`).

#### Step B: Create Macvlan Network

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my_macvlan_net
```

| Flag | Description |
|------|-------------|
| `-d` | Driver (shortcut) |
| `-o` | Options |
| `--subnet` | IP range for container assignment |
| `--gateway` | Router's address |
| `-o parent` | Physical network card |

#### Step C: Run Container

```bash
docker run -d \
  --name my_web_server \
  --network my_macvlan_net \
  --ip 192.168.1.50 \
  nginx
```

> [!warning] Important
> Always assign a **static IP** to avoid DHCP conflicts with other devices on your network.

---

### Troubleshooting Macvlan

| Issue | Solution |
|-------|----------|
| Host can't communicate with container | Create a "shim" (routing link) |
| Promiscuous mode required | Enable on network card/VM |
| IP conflicts | Use static IPs, check for duplicates |
| Wi-Fi issues | Macvlan doesn't work well on Wi-Fi — use Ethernet |

---

## 5. IPvlan Networking

**IPvlan** is similar to Macvlan but operates at the IP level instead of MAC address level. It provides a simpler model for containers that need direct network access.

### Key Differences from Macvlan

| Aspect | Macvlan | IPvlan |
|--------|---------|--------|
| **Level** | MAC address | IP address |
| **Complexity** | More complex | Simpler |
| **Host Communication** | Needs shim | Works out of the box |
| **Use Case** | Legacy, specific MAC needs | Modern, simple Layer 2/3 |

### Creating IPvlan Network

```bash
docker network create -d ipvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my_ipvlan_net
```

---

## 6. None Network

Disables all networking for the container:

```bash
docker run --network none my-image
```

**Use case:** Isolated containers that don't need network access.

---

## 7. Overlay Networking (Swarm)

**Overlay** networks connect containers across multiple Docker hosts. Used with **Docker Swarm** for cluster orchestration.

```bash
docker network create -d overlay my-overlay-net
```

> [!tip] Note
> For production clusters at scale, consider **Kubernetes** instead of Swarm.

---

## Network Commands Reference

| Command | Description |
|---------|-------------|
| `docker network ls` | List all networks |
| `docker network create` | Create a new network |
| `docker network rm` | Remove a network |
| `docker network inspect` | View network details |
| `docker network connect` | Connect container to network |
| `docker network disconnect` | Disconnect container from network |
| `docker network prune` | Remove unused networks |

---

## Network Type Comparison

| Network Type | Use Case | Port Mapping | IP Address | Isolation |
|--------------|----------|--------------|------------|-----------|
| **Bridge (default)** | Most apps | Required | Internal (172.17.x.x) | High |
| **Host** | High-performance apps | Not needed | Host IP | Low |
| **User-Defined** | Microservices | Required | Custom subnet | High |
| **Macvlan** | Legacy, direct LAN | Not needed | Real LAN IP | Low |
| **IPvlan** | Modern Layer 2/3 | Not needed | Real LAN IP | Low |
| **None** | Isolated containers | N/A | None | Maximum |
| **Overlay** | Multi-host (Swarm) | Required | Swarm-managed | High |

---

## Key Takeaways

- Docker provides **7 network types** for different use cases
- **Bridge** = default, good for standalone containers
- **User-Defined** = DNS-based service discovery, better isolation
- **Host** = highest performance, no isolation
- **Macvlan/IPvlan** = containers appear as physical devices on network
- **None** = completely isolated
- **Overlay** = multi-host networking (Swarm)
- Service names act as DNS in user-defined networks

---
